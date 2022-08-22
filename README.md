# About
Runs vstest for all projects in solution that match regex
Note that it works with convention that the *projectName*.dll exists within the containing proj folder
bin/*Release|Debug*/*projectName*.dll
or
bin/*Release|Debug*/*framework*/*projectName*.dll

# Inputs
solution - not required, will find the first .sln file in the github workspace

testNameRegex - defaults to /test/i

configuration - defaults to find

# Example workflow yaml
```
name: demo yaml for vstest-solution-action
on: # as desired
    pull_request: 
        types: [opened, synchronize, edited]
jobs:
  vstest-all-test-projects-in-solution:
    name: use required actions first
    runs-on: windows-2019
    steps:
      - name: checkout
        uses: actions/checkout@v2
      - name: set up msbuild
        uses: microsoft/setup-msbuild@v1.0.2
      - name: set up VSTest
        uses: darenm/Setup-VSTest@v1
      - name: nuget restore
        run: nuget restore DummyZipVsix.sln
      - name: build vsix
        run: |
          msbuild A.sln 
      - name: vs test solution
        uses: tonyhallett/vstest-solution-action@v1.0.0
```