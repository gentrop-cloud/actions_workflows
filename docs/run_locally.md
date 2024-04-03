## Running a workflow locally

In case any problem happens with the workflow, we can execute it locally before forcing the execution or pushing commits just to see it working. 
To run localy there's this great tool called Act https://github.com/nektos/act

To install it you just download it like:
`brew install act`

And to run a specif workflow is like:
`act -W .github/workflows/develop-functions-cd.yaml`

There are options for replacing github secrets, pass env variables, and much more, checkout its github docs for more. 

In case you can't get the project Firebase_Token, you can get a temporary one with:
`firebase login:ci`

