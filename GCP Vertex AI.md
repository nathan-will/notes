

VAI used currently with magenet
- infra code lives there(?)
- Py torch + lightning frontend
- WnB run locally (software downloaded and run against data locally, laptop or VM?)
- models stored in buckets currently > to model registry
- Primary goal ~2 yrs - internal use via notebooks
	- Also as an API, model called as an additional package within the CLI, then run on the given executor
- To use the model, once it's been check on a sub-dataset, `gcloud ai custom-jobs create` to launch the jobs on VAI
- Metrics/model monitoring - potential to use this? 


![[IMG_4453.jpg]]