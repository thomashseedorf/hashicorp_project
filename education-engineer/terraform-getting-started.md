# Getting Started with Terraform

Terraform is the most popular langauge for defining and provisioning infrastructure as code (IaC). Let's see how you can use Terraform to create and destroy your own infrastructure resources. Resources are the components of your infrastructure, such as an EC2 instance, Docker container image, or other component. See [Resources](https://developer.hashicorp.com/terraform/language/resources) to learn more.

## Prerequisites

- The latest version of Terraform
- The latest version of Docker
- Access to your system's command line

To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html) and download the appropriate zip archive for your system.

## Configure Terraform

After you install Terraform, create a directory for the configuration file.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Create a Terraform configuration file in the directory.

```shell
$ touch main.tf
```

Copy and paste this code into the file.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Save and close the file.

## Create and destroy resources

Initialize Terraform. Terraform installs the Docker provider specified in the code. 

Providers are Terraform plugins that allow Terraform to connect to other platforms and use APIs. See [Providers](https://developer.hashicorp.com/terraform/language/providers) for more information.

```shell
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of kreuzwerker/docker...
- Installing kreuzwerker/docker v2.25.0...
- Installed kreuzwerker/docker v2.25.0 (self-signed, key ID BD080C4571C6104C)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

Check for errors. If you see errors, check that you correctly copied and pasted the code.

If you don't see any errors, enter the command below, type `yes` at the prompt, and press ENTER to provision the resources. This command can take a few minutes to run.

```shell
$ terraform apply

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + gateway                                     = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ip_address                                  = (known after apply)
      + ip_prefix_length                            = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "training"
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + healthcheck {
          + interval     = (known after apply)
          + retries      = (known after apply)
          + start_period = (known after apply)
          + test         = (known after apply)
          + timeout      = (known after apply)
        }

      + labels {
          + label = (known after apply)
          + value = (known after apply)
        }

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id          = (known after apply)
      + image_id    = (known after apply)
      + latest      = (known after apply)
      + name        = "nginx:latest"
      + output      = (known after apply)
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.
╷
│ Warning: Deprecated attribute
│ 
│   on main.tf line 12, in resource "docker_container" "nginx":
│   12:   image = docker_image.nginx.latest
│ 
│ The attribute "latest" is deprecated. Refer to the provider documentation for
│ details.
│ 
│ (and one more similar warning elsewhere)
╵

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 6s [id=sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 4s [id=8dec00715c2e5f02fbd70d2cee75cc60787c1a91fae94edd44ad6979eebadba6]
╷
│ Warning: Deprecated attribute
│ 
│   on main.tf line 12, in resource "docker_container" "nginx":
│   12:   image = docker_image.nginx.latest
│ 
│ The attribute "latest" is deprecated. Refer to the provider documentation for
│ details.
│ 
│ (and one more similar warning elsewhere)
```

Destroy the infrastructure resources.

```shell
$ terraform destroy

docker_image.nginx: Refreshing state... [id=sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52nginx:latest]
docker_container.nginx: Refreshing state... [id=8dec00715c2e5f02fbd70d2cee75cc60787c1a91fae94edd44ad6979eebadba6]

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach                                      = false -> null
      - command                                     = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - container_read_refresh_timeout_milliseconds = 15000 -> null
      - cpu_shares                                  = 0 -> null
      - dns                                         = [] -> null
      - dns_opts                                    = [] -> null
      - dns_search                                  = [] -> null
      - entrypoint                                  = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env                                         = [] -> null
      - gateway                                     = "172.17.0.1" -> null
      - group_add                                   = [] -> null
      - hostname                                    = "8dec00715c2e" -> null
      - id                                          = "8dec00715c2e5f02fbd70d2cee75cc60787c1a91fae94edd44ad6979eebadba6" -> null
      - image                                       = "sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52" -> null
      - init                                        = false -> null
      - ip_address                                  = "172.17.0.2" -> null
      - ip_prefix_length                            = 16 -> null
      - ipc_mode                                    = "private" -> null
      - links                                       = [] -> null
      - log_driver                                  = "json-file" -> null
      - log_opts                                    = {} -> null
      - logs                                        = false -> null
      - max_retry_count                             = 0 -> null
      - memory                                      = 0 -> null
      - memory_swap                                 = 0 -> null
      - must_run                                    = true -> null
      - name                                        = "training" -> null
      - network_data                                = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.2"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode                                = "default" -> null
      - privileged                                  = false -> null
      - publish_all_ports                           = false -> null
      - read_only                                   = false -> null
      - remove_volumes                              = true -> null
      - restart                                     = "no" -> null
      - rm                                          = false -> null
      - runtime                                     = "runc" -> null
      - security_opts                               = [] -> null
      - shm_size                                    = 64 -> null
      - start                                       = true -> null
      - stdin_open                                  = false -> null
      - stop_signal                                 = "SIGQUIT" -> null
      - stop_timeout                                = 0 -> null
      - storage_opts                                = {} -> null
      - sysctls                                     = {} -> null
      - tmpfs                                       = {} -> null
      - tty                                         = false -> null
      - wait                                        = false -> null
      - wait_timeout                                = 60 -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id          = "sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52nginx:latest" -> null
      - image_id    = "sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52" -> null
      - latest      = "sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:0047b729188a15da49380d9506d65959cce6d40291ccfb4e039f5dc7efd33286" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.
╷
│ Warning: Deprecated attribute
│ 
│   on main.tf line 12, in resource "docker_container" "nginx":
│   12:   image = docker_image.nginx.latest
│ 
│ The attribute "latest" is deprecated. Refer to the provider documentation for
│ details.
╵

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=8dec00715c2e5f02fbd70d2cee75cc60787c1a91fae94edd44ad6979eebadba6]
docker_container.nginx: Destruction complete after 1s
docker_image.nginx: Destroying... [id=sha256:1403e55ab369cd1c8039c34e6b4d47ca40bbde39c371254c7cba14756f472f52nginx:latest]
docker_image.nginx: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.


```

To confirm you want to destroy the resources, type `yes` and press ENTER. Terraform destroys the resources it created earlier.

## Next steps

To learn more, see the tutorials on how you can [change infrastructure](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-change), [define input variables](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-variables), and more. You might also consider asking a question and receiving help in the [Community Forum](https://discuss.hashicorp.com/c/terraform-core/27).
