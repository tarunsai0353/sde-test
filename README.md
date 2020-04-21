# SDE Online Assessment

The purpose of this test is to evaluate your:

- development skills in a production environment
- ability to provide a solution given a specification using best practices
- design thinking
- coding style
- ability to containerize a solution using Docker

You may use any language you are comfortable with, as long as it is quick and simple to execute in Docker.

## Problem description

Given a JSON file which will define an array of **bond** objects (of arbitrary size), write a **command line tool** to calculate the **spread** between each **corporate bond** and the nearest **government bond** benchmark, save these results in a JSON file, and express the spread in **basis points**, or **bps**. If any properties are missing from a bond object, do not include it in your calculations or output.

**Spread** is defined as the difference between the yield of a corporate bond and its government bond benchmark. 

A government bond is a good **benchmark** if it is as close as possible to the corporate bond in terms of **years to maturity**, also known as **term** or **tenor**. 

If there is a *tie* for closest government bond by **tenor**, break the tie by choosing the government bond with the *largest* **amount outstanding**. 

To convert your difference to **basis points**, just scale your spread by 100 and display as an integer (truncate trailing decimals), e.g. if your spread comes out to 2.127, this will be expressed in your output file as "212 bps".

### Sample input

```json
{
  "data": [
    {
      "id": "c1",
      "type": "corporate",
      "tenor": "10.3 years",
      "yield": "5.30%",
      "amount_outstanding": 1200000
    },
    {
      "id": "g1",
      "type": "government",
      "tenor": "9.4 years",
      "yield": "3.70%",
      "amount_outstanding": 2500000
    },
    {
      "id": "c2",
      "type": "corporate",
      "tenor": "13.5 years",
      "yield": null,
      "amount_outstanding": 1100000
    },
    {
      "id": "g2",
      "type": "government",
      "tenor": "12.0 years",
      "yield": "4.80%",
      "amount_outstanding": 1750000
    }
  ]
}
```

### Sample output

```json
{
  "data": [
    {
      "corporate_bond_id": "c1",
      "government_bond_id": "g1",
      "spread_to_benchmark": "160 bps"
    }
  ]
}
```

### Explanation

Each output object in the list represents a pairing of one corporate bond to its closest government bond benchmark, and the spread between their yields.

The best benchmark for bond `c1` is `g1`, since the absolute difference in their terms (|10.3 - 9.4|) is only 0.9, but comparing `c1`and `g2` gets you 1.7. The spread is calculated as simply the corporate yield - government yield, you would obtain 5.30 - 3.70 = 1.60, which you must represent in basis points as "160 bps".

The bond `c2` is not included in the output because it is missing a property, `yield`. If *any* properties are missing from a bond object, *do not include it in the calculation and output*. You may assume you will always have at least one valid government bond and at least one valid corporate bond, for all inputs.

## Command line interface

To make this challenge easier to work on, and easier for us to evaluate, you must implement a specific command line interface for your program, *regardless of which language you use*. For example, let the name of your executable be `sde-test-solution`, then your program should accept and use the following arguments only.

`$ sde-test-solution input_file.json output_file.json `

The **first argument** will be the path to the input JSON file (has the input bond data), formatted as the example above. Your program must read the JSON file at this path and process it.

The **second argument** will be the path where your program must write the output of your program, as a JSON file, formatted as the example above.

You may assume that both arguments will always be provided during our testing, and that the paths will always be valid.

## Input file format

See `sample_input.json`, or example above. You may assume that all properties or fields will be present and valid.

## Output file format

See `sample_output.json`, or example above. You must follow the file structure exactly, and include all fields for each output object shown in the example.

## Further requirements

### Docker

You must **create and fill out a working** `Dockerfile` in the directory of your solution, that will handle the building and dependencies for your final executable.

Your final executable should be setup as an entrypoint so that we can automatically pass in the paths for your program to process input from and write output to.

You must set your `WORKDIR` to be `/submission`, make sure to copy your files there, in your container. This is important because we will be setting up volumes on our end, and would like to reason about your file system.

If the build fails, or the Dockerfile does not accept arguments to your program (e.g. via `docker run sde-test-image input_file.json output_file.json`), then **we will be unable to grade your submission** for correctness.

### Testing

Code in production should be tested, as this guarantees expected behaviour and functionality, as well as  making it safer to modify implementations while knowing behaviour remains the same. Do your best to **write some insightful automated tests** for your command line program.

### Documentation

**Replace this README file with your own**. Write documentation on your submission such that someone can read it and immediately understand CLI arguments, inputs and outputs, behaviours, and reasoning behind design decisions that went into your solution, and any trade-offs. Discuss the run time complexity of your main calculation.

## Evaluation

Upon receiving your submission, we will evaluate:

- **Functionality**: does the Docker image build and execute? Does the program pass our automated test cases?
- **Code style**: is the code organized well and easy to understand? Are there any smells or severe formatting issues? Is it efficient?
- **Testing**: are there any automated tests written? Do they provide sufficient coverage?
- **Documentation**: is a new README present? Is it clearly expressed and well written? Does it touch on all topics mentioned?

## Submission

Clone this repository and make commit the work to your own repo to share with us. If you decide to make the repo private, add @overbondeng as a collaborator, so that we can clone and view your solution.

Alternatively, email us a zip file containing your entire solution.
