name: 'Test for GA'
description: 'testing envs'
inputs:
  who-to-greet:  # id of input
    description: 'Testing'
    required: true
    default: 'World'
outputs:
  random-joke: # id of the output
    description: "Random joke"
    value: ${{ steps.random-joke-generator.outputs.g-env }} #gets the value from line 18. Syntax: steps.<id>.outputs.<name>
env:
  VERTEX_CONTAINER: 'gcr.io/deeplearning-platform-release/base-cu110:latest'
  VERTEX_JOB_URI: 'https://console.cloud.google.com/vertex-ai/locations'
  VERTEX_NOTEBOOK_URI: 'https://notebooks.cloud.google.com/view'
runs:
  using: "composite"
  steps:
    - run: echo Hello ${{ inputs.who-to-greet }}.
      shell: bash
    - id: random-joke-generator
      # run: echo "::set-output name=g-env::$(echo "${VERTEX_CONTAINER}")"
      run: echo "::set-output name=random-number::$(echo $RANDOM)"
      shell: bash
    - run: echo "${{ github.action_path }}" >> $GITHUB_PATH
      shell: bash          
    - run: goodbye.sh
      shell: bash
    - run: |
          for i in {1..5}
          do
          echo "${VERTEX_CONTAINER}"
          done
      shell: bash