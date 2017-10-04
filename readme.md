This repository defines a docker image with Ansible installed.

# Usage
You can use this image in two ways:  

1. As a base image for another container
2. Interactively

## Base Image
This container can be used as a base image for another container. Doing so allows you to pre-package a playbook and it's environment and context. This is useful if you want to run the same playbook automatically, for example during Continuous Integration or Deployment, or call it from another script.

To use this image as a base for your container:
1. Define it as your base image in your dockerfile.  
`FROM synaxa/ansible:latest`

2. Add your playbook to the image  
`ADD . /ansible/playbooks`
> _Note:  
> Ensure that your playbook is in the same root directory as your dockerfile, or modify the paths in the ADD command_

3. (Optional) Add your ansible configuration  
`ADD ansible.cfg /etc/ansible/ansible.cfg`

4. Set your container's entrypoint  
`ENTRYPOINT ["ansible-playbook", "playbook.yml"]`

> _Note:  
> Substitute `playbook.yml` for your playbook's filename_

5. Build and run your image  
`docker run -i <your image>`

## Interactive
This container is intended to be used as a base image for another container, but you can also use it interactively. To do so, execute the following steps:

1. Start the container   
`docker run -it --rm synaxa/ansible`

2. Execute an ansible command 
`ansible all -i hosts playbook.yaml`

## Additional Info
1. You can modify your playbook and container to allow a greater degree of flexibility when using it. For example, you can write a playbook which utilizes Jinja2 templating to define playbook parameters, and then pass those parameters in to the container by appending the `--extra-vars` ansible flag.

2. You can enable debugging in your container by setting `ENV ANSIBLE_DEBUG True` in your dockerfile. The base image writes debug logs to `/ansible/logs` when this variable is set. You can retreive these logs on the host by mounting a volume which maps to the `/ansible/logs` directory in the container. Additionally, you may need to run ansible with verbosity (`-vvvv`).