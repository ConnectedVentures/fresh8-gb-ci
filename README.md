# Fresh8 gb continuous integration
[![Build Status](https://travis-ci.org/ConnectedVentures/fresh8-gb-ci.svg?branch=master)](https://travis-ci.org/ConnectedVentures/fresh8-gb-ci)

Fresh8-gb-ci is a library in ongoing development to provide an easily integrated continuous integration solution for gb projects. We created this due to our large amount of micro services all requiring a separate, or slightly different, CI script. This tool can be cloned and run as part of your continuous integration testing process using a lightweight config file in the root of the project.

Currently supports the following commands:

* gocode test -cover
* golint
* go vet

## Installation

### Config.yaml

Take a copy of `config.yaml.example` from the repository, or copy it from the example below and place it in the root of your project as `ci_config.yaml`. If your project is `gb` based this will be alongside your `src` and `vendor` directories.

**Note**: We now support `gb` and `glide` projects.

The packages list should include all of the base packages you want to test, the tests are run recursively so there’s no need to include every inner package.

You may also ignore test commands, for example to ignore code coverage add "code_coverage" to the ignored_commands array.

**Note**: Packages should not include a trailing slash.

`ignored_packages` should include a list of the packages you don’t want to be tested, let’s say they’re actively in development and changing a lot you may not want the test coverage tool running every time.

`code_coverage: threshold` is the % of test coverage a given package must be above to pass, this must be a decimal based value (as per example).

`all: packages` can be marked as `all` to test everything. This will exclude the vendor.

#### Example file

```YAML
all:
  packages:
   - "./your_cool_package"
  project_type: "gb"
  ignored_commands: []

code_coverage:
  threshold: 80.0
  ignored_packages:
    - "your_cool_package/neat_inner_package"

go_vet:
  ignored_packages: []

golint:
  ignored_packages: []

```

### Codeship

To integrate with codeship, first add your `config.yaml` as per all systems, then add the following commands to your `Test Pipeline` for the project.

```bash
git clone --branch 1.0.0 https://github.com/ConnectedVentures/fresh8-gb-ci

# For gb
env GOPATH=$(pwd):$(pwd)/vendor python fresh8-gb-ci/ci.py

# For glide 
python fresh8-gb-ci/ci.py
```

You will also need to install a few dependencies, add the following to your `Setup Commands`

```bash
go get -u github.com/golang/lint/golint

pip install pyyaml
```

## Roadmap

* Static code analysis: use of init functions. (Pull Request pending)
* Ignore immediately following line (`// f8-ignore: [ init ]`) for cases when usage is unavoidable.
* Static code analysis: use of function calls in package level variable declaration. (e.g `var x = y()`)
* Tidy up into a more class based structure, reduce code reuse.
* Test other CI platforms than codeship.
* Non gb projects.
* Investigate build warnings rather than outright fails for CI platforms that support it.

## Contributing

That would be awesome.

Please include a brief description of what your pull request does, we are trying to keep dependencies to a minimal so there will need to be a good reason for additional packages / tools to be installed.

We use [Flake8](https://pypi.python.org/pypi/flake8) for our linting / formatting, any pull requests that don’t conform to this won’t be accepted, sorry! You can find linters for most editors to check this.

### License

We use the MIT license, you can find it in LICENSE.md.
