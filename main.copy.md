on: [push]

env:
  DAY_OF_WEEK: Monday
  First_Name: "Alex"
  VERTEX_JOB_URI: 'https://console.cloud.google.com/vertex-ai/locations'

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      - uses: actions/checkout@v3
      - id: foo
        uses: ./.github/actions/hello-world-composite-action # uses action in same repo
       # uses: alexdemichieli/hello-world-composite-action@v4 # would use action in another repo
        with:
          who-to-greet: 'Alex'
      # - run: echo here's a joke ${{ steps.foo.outputs.random-joke }} #steps foo should 'output' random-joke. Random-joke is the id of the output in the action.yml file
      - run: echo "use of global variable in checkmark output ${{env.DAY_OF_WEEK }}" #to access global vars you need to access the global context with .env and ${{}}
      - run: echo " this is the variable in log ${VERTEX_JOB_URI}"
        shell: bash

      - name: "Another env test without curlies ${{env.First_Name}}! or $First_Name"
        run: echo "$First_Name"
        env:
          First_Name: Mona

# name: Greeting on variable day

# on:
#   workflow_dispatch

# env:
#   DAY_OF_WEEK: Monday

# jobs:
#   greeting_job:
#     runs-on: ubuntu-latest
#     env:
#       Greeting: Hello
#     steps:
#       - name: "Say Hello Mona it's Monday"
#         run: echo "$Greeting $First_Name. Today is $DAY_OF_WEEK!"
#         env:
#           First_Name: Mona