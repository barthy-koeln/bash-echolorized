# Script IO: Pretty and minimal bash output

This library helps you write prettier scripts for CI/CD, git hooks, ...

## Installation

You can require the package via composer for PHP projects, or npm/yarn for Browser/Node projects.

```shell
composer require --dev barthy-koeln/bash-echolorized
```

```shell
yarn add -D bash-echolorized
```

## Importing & usage

If this package is required via composer:

```shell
#!/usr/bin/env bash

DIR=$(realpath "$(dirname "${BASH_SOURCE[0]}")")

# if required via composer
source "$DIR/vendor/bin/echolorized"

# if required via yarn/npm
source "$DIR/node_modules/.bin/echolorized"
```

### Basic usage:

```shell
e_info "Checking Twig Code"
e_success "All done"
e_warning "Careful!"
e_error "That did NOT work."
```

Output:

![Baisc usage output](./output_standard.png)

### Advanced usage (git pre-commit example):

```shell
colored_output "$GREEN" "\n[EXECUTING PRE COMMIT HOOK]"
e_info "USE ${YELLOW}git commit -n -m 'your message'${RESET} to bypass these checks."
echo -e ''

e_info "Checking YAML config"

if ! "$DIR/console" --quiet lint:yaml config; then
  e_error "Error in YAML config. Run ${YELLOW}bin/console lint:yaml${RESET} to check.\n"
  exit 1
fi

e_success "All good, staging additional changes.\n"
git update-index --again
```

Output:

![Advanced usage output with error](./output_advanced-error.png)
![Advanced usage output with success](./output_advanced-success.png)
___


```shell
tagged_output "$YELLOW" "BUILD" "Fetching dependencies"
tagged_output "$YELLOW" "BUILD" "Building bundles"
tagged_output "$YELLOW" "BUILD" "Making binaries executable"
tagged_output "$GREEN" "TEST" "Tests are green" " "

tagged_output "$RED" "¯\_(ツ)_/¯" "Dunno..?\n" "\n"

tagged_output "$YELLOW" "DIRECTORY" "src"
tagged_output "$GREEN" "FILE" "README.md" "\t"
tagged_output "$GREEN" "FILE" "package.json" "\t"
tagged_output "$GREEN" "FILE" "composer.json" "\t"
```

Output:

![Advanced usage output with error](./output_tagged.png)

### Methods:

```shell
colored_output() {
  # $1: Color
  # $2: Text
}
```

```shell
tagged_output() {
# $1: Color
# $2: Tag
# $3: After tag
# $4: Before tag (tabs, newlines, ...)
}
```
