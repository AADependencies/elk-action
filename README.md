# elk-action

## Usage
Action to send data from other actions to ELK microservice that authenticates the request and pushes it to ELK instance. Best used as an embedded action inside of a composite action

## Inputs
```yaml
Required: 
    action_name: Name of action calling elk-action
    elk_endpoint: Endpoint to use for ELK microservice

Optional:
    start_time: Start time of the calling action (must be generated in the calling action)
```

## Sample Use
```yaml
name: AWESOME ACTION
description: An action used for awesomeness

runs:
  using: "composite"
  steps: 

  - name: Set Start time
    id: start
    run: |
      echo ::set-output name=start-time::$(date +'%Y-%m-%dT%H:%M:%S-0700')
    shell: bash

  - name: run Awesome Action
    uses: Awesome-Action

  - name: Send data to ELK
    uses: ./.github/actions/ELK-Action
    with:
      action_name: "Awesome-Action"
      start_time: ${{steps.start.outputs.start-time}}
      elk_endpoint: "${{secrets.elk-url}}"
```