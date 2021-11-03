# README for dev

## Introduce

  Clone, Build and debug

  Terraform Sample: [tfexample-vmworkstation](https://github.com/huhongjun/tfexample-vmworkstation)

## prerequisites

  Ubunt 20.04
  sudo apt-get install build-essential
  sudo apt-get install -y golang-go

## Cloning the Project, then build

  Makefile line 23, 
```  
  @GOOS=linux CGO_ENABLED=0  go build -o $(BINARY) $(SOURCE)
```
  Build the binary
~~~
  git clone git@github.com:elsudano/terraform-provider-vmworkstation
  //CGO_ENABLED=0
  make clean
  make build
~~~
## Debbuging

  config local development by config file dev.tfrc
```
  provider_installation {

    # Use /home/axe/workshop/terraform-provider-vmworkstation/release as an overridden package directory
    # for the hashicorp/null provider. This disables the version and checksum
    # verifications for this provider and forces Terraform to look for the
    # null provider plugin in the given directory.
    dev_overrides {
      "elsudano/vmworkstation" = "/home/axe/workshop/terraform-provider-vmworkstation/release"

    }

    # For all other providers, install them directly from their origin provider
    # registries as normal. If you omit this, Terraform will _only_ use
    # the dev_overrides block, and so no other providers will be available.
    direct {}
  }
```

terraform test
```
  export TL_DEBUG=true
  export TF_LOG=TRACE # INFO, DEBUG, TRACE
  export TF_LOG_PATH="terraform.log"

  //local development provider
  export TF_CLI_CONFIG_FILE=${PWD}/dev.tfrc
  // vmws debug for the provider
  export VMWS_DEBUG=true

  terraform init
  terraform validate
  terraform plan

  rm terraform.log
  //rm /home/axe/vmware/vm-tf1 -rf
  terraform apply -auto-approve
```