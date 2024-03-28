## Common Workflows 


### Lint 
To help us to stop using pre-commit hooks, and stop not detected linting issues in the middle of the project.
This workflow help us to detect Linting issues with annotations in the PR, and it fixes other issues auto. 

The awesome action wearerequired/lint-action@v2 is available for sereval languages, but here it is impoved to use within our TS projects.

### Test 
To help to run our suit tests, and writing the workflow only once, we could use this one. 
It should be extensible, accepting parameters like `use_emulators` and specify which script should be run. 

If the test scenario is too different, we can always create a new one from the present configuration and specify it on our project.


### Deploy

Ref: https://cloud.google.com/blog/products/identity-security/enabling-keyless-authentication-from-github-actions
