# Terraform

[Terraform](https://www.terraform.io) is a cloud native, open-source infrastructure provisioning tooling similar to Ansible. IOS XE Terraform utilizes RESTCONF + YANG to configure devices using a single binary file. Terraform is declarative, meaning that it defines the desired state. It has commercial support from HashiCorp.

Terraform providers communicate with the desired resource. Typically, there are one or more resources in a .tf file.

Terraform is installed using the “apt” package management system. Running the Debian Package command, or dpkg, with the -L flag (list) shows which packages are installed on the system, including the Terraform tool.

## CLI2YANG
In Cisco IOS XE Cupertino 17.7.1 and later releases, you can automatically translate IOS commands into relevant NETCONF-YANG XML or RESTCONF-JSON request messages. You can analyze the generated configuration messages and familiarize with the Xpaths used in these messages. The generated configuration in the structured format can be used to provision other devices in the network; however, this configuration cannot be modified.

## Retrieve running config on the device in CLI
Review the CLI running configuration

1. Review the running configuration in the good ole fashioned CLI using `show run`


<!-- ## Retrieve running config formatted in XML for NETCONF
Generate the XML of the current running config using `netconf-xml`

![](./imgs/cli_to_xml.gif) -->


## Retrieve run config formatted with JSON for RESTCONF
Generate the JSON of the current running config using  `show run | format restconf-json`

![](./imgs/cli_to_json.gif)

1. Notice the formatting of VLANs. Now, we'll use the JSON in a Terraform file to configure a new VLAN on a Catalyst 9300.
1. Navigate to the terraform directory `cd ~/terraform`
1. Review the terraform.tf file to apply a new VLAN to the switch. `cat terraform.tf`, which will look similar to the following


### terraform.tf
```
terraform {
  required_providers {
    iosxe = {
      source = "CiscoDevnNet/iosxe"
    }
  }
}

provider "iosxe" {
  host = "https://10.1.1.5"
  insecure = true
  device_username = "admin"
  device_password = "Cisco123"
}

resource "iosxe_rest" "vlan_example_put" {
  method = "PUT"
  path = "/data/Cisco-IOS-XE-native:native/vlan/vlan-list=511"
  payload = jsonencode(
    {
    "Cisco-IOS-XE-vlan:vlan-list": {
          "id": "511",
          "name": "VLAN511-flag8838384747"
      }
    }
  )
}

resource "iosxe_rest" "vlan_example_get" {
  method = "GET"
  path = "/data/Cisco-IOS-XE-native:native/vlan"
}
```

## Apply Terraform
Now that the .tf file has been reviewed and is ready for use, the Terraform tool itself can be initialized and then used to apply this configuration
![](./imgs/terraform.gif)


1. Initialize Terraform with `terraform init`
1. Apply the configuration with the terraform apply command `terraform apply -auto-approve`
![](./imgs/terraform_init_and_apply.png)

1. Note that the terraform provider has been executed if the message appears "Apply complete! Resources: 2 added, 0 changed, 0 destoyed."
![](./imgs/terraform_apply_complete.png)