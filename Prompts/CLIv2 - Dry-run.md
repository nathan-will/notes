
Nathan:
With the `--dry-run`, I've been racking my brains on the best way to implement this for an hour, at first I was thinking just build on duet_validate_inputs.go, however input paths will likely be part of all pipelines, and at least one container will be too. Also if I'm reading the code correctly those validation functions are called as standard to prevent nextflow starting then reporting the failure?As such, is it better to create a more global ~~controller~~/ service/ repo for this `--dry-run` validation? We can then work this into existing controllers for given pipelines


Etienne:
I've implemented it that way in Duet: https://github.com/cegx-ds/biomodal_go_cli/blob/f6c11f9407093ec9ec34ade39b0801d082a3dddb/biomodal/controllers/run_controller/run_duet_controller/duet_controller.go#L93-L101
basically, it's calling a separate method in the service.
This method has two parts: https://github.com/cegx-ds/biomodal_go_cli/blob/main/biomodal/services/pipeline_service/duet_dry-run.go
some bespoke validation logic (e.g. check if file exist, if you can connect to registry, etc.)
"fake" launch the actual pipeline (with the true as second argument, which makes the repository not launch the pipeline — cf. https://github.com/cegx-ds/biomodal_go_cli/blob/f6c11f9407093ec9ec34ade39b0801d082a3dddb/biomodal/repositories/nextflow_repository/nextflow_repository.go#L74-L78))

^^ from my colleague.

For this ticket, we want to add the logic to evaluate whether a command will work, before it is run. To do this we need to check the folder exists, and for the value of --input we check it has files matching *.fastq

For --output, that there is a directory that can be written to

The output folder can store “input size * 2”

Files that might need to be changed:

controllers:

services:

repositories:


### Architecture




### Testing
```
biomodal run duet \
--input /path/that/does/not/exist \
--output test_data/duet/1.4.1/ \
--tag test \
--mode 5bp \
--dry-run

  

# Test with directory without FASTQ files

mkdir /tmp/empty_dir

biomodal run duet \
--input /tmp/empty_dir \
--output test_data/duet/1.4.1/ \
--tag test \
--mode 5bp \
--dry-run
```