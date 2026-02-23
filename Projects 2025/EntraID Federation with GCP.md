# Entra ID Federation

## Intro

Presently the company pays ~$5000 for Google Workspace licenses per annum, these function mainly to provide access to the Google Cloud Platform. Secondarily, Google Auth sign-in is also used to access the following applications:

- Miro
- Tower/ Seqera Platform
- Google Colab
- Timely (though 365 federation is available and should work for all)
- DoIT

Google Groups are also relied on for RBAC.

Along with the cost saving, there is an additional motivation to streamline access control by using MS Entra ID for RBAC. Presently, RBAC is defined primarily by team, with relevant teams given access on a per-project basis.

### Potential blockers:


### Open questions:

Question:
Can we save money this way, having only Azure user accounts, thus enabling the removal of Google Workspace accounts?

Answer:
Federation is possible with free Cloud Identity accounts. Worth noting that users will lose access to Drive, Sheets etc. when the license is downgraded. Given that this is a possibility, it seems like we could save money, currently licenses are assigned to all users, however seems like this can be disabled, so we can [remove the auto assign](https://support.google.com/a/answer/1727173?hl=en#zippy=%2Cautomatically-assign-licenses-to-all-users), and downgrade a users licence to test.

Note: Tested with my own users, however cannot see the option to turn auto-licensing off, perhaps check with IT.

Question:
Will federation affect existing GCP user accounts SSO sign-in?

Answer:
> If the user enters an email address that belongs to a Cloud Identity or Google Workspace account that is configured to use single sign-on with Microsoft Entra ID, the user is then redirected to the Microsoft Entra ID sign-on screen.

As we use SSO with MS, it seems likely users would be redirected for the MS SSO screen rather than Google.

Question:
Will groups with the @cegx.co.uk domain be affected if this domain is not supported in Entra/ Azure?

Question:
Does on-prem sync affect the implementation at all? If the local server is down, are we locked out? Would VPN be needed to connect?

Question:
Would adding external users to project resources be affected?

Question:
Following federation, would users see an uptick in Google Cloud notification emails? Presently, mail in Google Workspace is not configured (as can only be present in either Azure or Google).

## Mapping

#### Single Tenant

We use a single tenant in Azure currently (TBC with IT)

> Map users
> The third factor to look at when planning to federate Microsoft Entra ID and Google Cloud is how to map users between Microsoft Entra ID and Cloud Identity or Google Workspace.
> 
> Successfully mapping Microsoft Entra ID users to users in Cloud Identity or Google Workspace requires two pieces of information for each user:
> 
> A stable, unique ID that you can use during synchronization to track which Microsoft Entra ID user corresponds to which user in Cloud Identity or Google Workspace.
> An email address for which the domain part corresponds to a Cloud Identity primary, secondary, or alias domain. Because this email address will be used throughout Google Cloud, the address should be human-readable.
> Internally, Microsoft Entra ID identifies users by ObjectId, which is a randomly generated, globally unique ID. While this ID is stable and therefore meets the first requirement, it's meaningless to humans and doesn't follow the format of an email address. To map users, the only practical options are to map by UPN or by email address.

Process:
- Once federation access has been set-up between the two, we must decide how users will be mapped
- It seems like UPN is the best option here for full integration
- Map users; refer to [mapping identity sets](https://docs.cloud.google.com/architecture/identity/best-practices-for-federating#mapping_identity_sets). We will want to enable access for a subset of users rather than the whole org. which is done by user assignment or scope filter.
- Map groups; these should be assigned from Entra ID, Google Groups may need to be recreated in Entra ID as security groups (with same email) for effective mapping.


Preliminary To-Do:
- Look at [enabling Cloud Identity](https://docs.cloud.google.com/identity/docs/how-to/set-up-cloud-identity-admin) - can we move users from workspace to Cloud ID Free?
- If we proceed, users will need to back-up data in Google Workspace apps
- Ensure we have a 1-to-1 mapping of google Groups in Entra/ AD (link to requirement)
- Move users from Google Workspace to Cloud Identity
- Create onboarding plan
- Prepare Cloud ID for federation
