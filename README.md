# Ansible Role: Kubernetes on Hetzner Cloud

This Ansible role is heavily based on the Kubernetes Install Tutorial from Christian Beneke <c.beneke@wirelab.org> (see reference link).

Features:
* supports Ubuntu/Debian based Linux OS (more to come)
* role also installs Docker dependency
* supports single and multi-master control planes
* uses Flannel for CNI
* supports Hetzner Cloud volumes
* supports MetalLB using Floating IP(s)
* supports switching of Floating IP to different worker node if current node becomes not ready
* supports Helm2 and Helm3 (only Helm2 tested so far)
* using Ansible Vault for Hetzner Cloud API token highly recommended

## Example Playbook

```yaml
- hosts:
    - kube01-master01
    - kube01-master02
    - kube01-master03
    - kube01-worker01
    - kube01-worker02
    - kube01-worker03

  vars:
    networking_floating_ip: 'A.B.C.D'
    kubernetes_hetzner_floating_ip: "{{ networking_floating_ip }}"
    
    kubernetes_hetzner_docker_version: '5:18.09.9~3-0~ubuntu-bionic'
    kubernetes_hetzner_kubernetes_version: '1.16.8'
    
    kubernetes_hetzner_api_token: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      [...]
    kubernetes_hetzner_network_id: <NetworkID>
    kubernetes_hetzner_cluster_name: kube01
    kubernetes_hetzner_dns_domain: kube01.dmsp.de
    kubernetes_hetzner_control_plane_endpoint: kube01.dmsp.de:6443
    kubernetes_hetzner_init_node: kube01-master01
    kubernetes_hetzner_helm_version: 2
    kubernetes_hetzner_install_metallb: True
    # only required for testing (quick re-install)
    kubernetes_hetzner_reset_cluster: False
    [...]
    # only for masters (e.g. in file group_vars/kube01-master.yml)
    kubernetes_hetzner_role_master: True

  roles:
    - ...
    - kubernetes-hetzner
```

## References

* https://community.hetzner.com/tutorials/install-kubernetes-cluster

## LICENSE

BSD 2-Clause License

## Author Information

This role was created in 2020 by Stefan Pommerening <step@dmsp.de>, https://www.dmsp.de
