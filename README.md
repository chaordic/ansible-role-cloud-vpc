cloud-vpc
=========

[![Project Status: Concept - initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)

Ansible role to manage all resources in Virtual Private Cloud Infrastructure.
from service provider - now we are supporting AWS (please help us to improve =] ).

Roadmap for Cloud Providers:
* AWS
* Microsoft Azure

Main roles to manage:
* Facts
* VPC
* Subnets
* Internet Gateway
* Route tables
* Service Endpoints

Requirements
------------

* python >= 2.7
* ansible >= 2.5

Specific for Cloud Provider:

* AWS
  1. boto
  1. boto3

* Azure
  1. azure >= 2.0.0

Role Variables
--------------

`cloud_vpc` : list of VPC Networks to be created. The map could be different between cloud providers, see examples bellow.

Dependencies
------------

- AWS Account with permissions to create VPC
- [AWS Credentials exported to be used by boto](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html)

Example Playbook
----------------

* Setup Cloud Network configuration for multi-cloud environment (AWS and Azure):

      - hosts: servers
        vars:
          ---
          #########################
          # Networks
          # 10.0.0.0/12 and 10.1.0.0/12
          #
          ## AWS
          ## 10.0.0.0/16 (10.0.0-255.0/16)
          ### us-east-1: 10.0.0.0/19 (sample bellow w/ max 32 blocks /24)
          ### us-east-2: 10.0.32.0/19
          ### ---------: 10.0.64.0/19
          ### ---------: 10.0.95.0/19
          ### ---------: 10.0.128.0/19
          ### ---------: 10.0.160.0/19
          ### ---------: 10.0.192.0/19
          ### ---------: 10.0.224.0/19
          ## Azure
          ### 10.1.0.0/16
          ### us-east-1: 10.1.0.0/19 (sample bellow)
          #########################
          cloud_vpc:
          - name: vpc_use1_aws
            block: 10.0.0.0/19
            provider: aws
            region: us-east-1
            igw: yes
            nat_gw: no
            nat_gw_subnet: net_natgw_public
            security_groups: "{{ security_groups | d([]) }}"
            routes:
              - name: rt_private
                table:
                  - dest: 0.0.0.0/0
                    gateway_id: natgw
              - name: rt_public
                table:
                  - dest: 0.0.0.0/0
                    gateway_id: igw
              # - name: rt_natgw
              #   table:
              #     - dest: 0.0.0.0/0
              #       gateway_id: igw
            subnets:
              - name: net_nodes_public_1
                az: us-east-1b
                cidr: 10.0.0.0/24
                route: rt_public
                public_ip: true
              - name: net_nodes_private_1
                az: us-east-1b
                cidr: 10.0.1.0/24
                route: rt_private
              - name: net_nodes_public_2
                az: us-east-1c
                cidr: 10.0.2.0/24
                route: rt_public
                public_ip: true
              - name: net_nodes_private_2
                az: us-east-1c
                cidr: 10.0.3.0/24
                route: rt_private
              - name: net_nodes_public_3
                az: us-east-1d
                cidr: 10.0.4.0/24
                route: rt_public
                public_ip: true
              - name: net_nodes_private_3
                az: us-east-1d
                cidr: 10.0.5.0/24
                route: rt_private
              - name: net_nodes_public_4
                az: us-east-1e
                cidr: 10.0.6.0/24
                route: rt_public
                public_ip: true
              - name: net_nodes_private_4
                az: us-east-1e
                cidr: 10.0.7.0/24
                route: rt_private
              - name: net_elb_public_1
                az: us-east-1b
                cidr: 10.0.10.0/24
                route: rt_public
                public_ip: true
              - name: net_elb_public_2
                az: us-east-1c
                cidr: 10.0.11.0/24
                route: rt_public
                public_ip: true
              - name: net_elb_public_3
                az: us-east-1d
                cidr: 10.0.12.0/24
                route: rt_public
                public_ip: true
              - name: net_elb_public_4
                az: us-east-1e
                cidr: 10.0.13.0/24
                route: rt_public
                public_ip: true
              - name: net_vpn_public_1
                az: us-east-1b
                cidr: 10.0.29.0/24
                route: rt_public
                public_ip: true
              # - name: net_vpn_public_2
              #   az: us-east-1e
              #   cidr: 10.0.30.0/24
              #   route: rt_public
              #   public_ip: true
              # - name: net_natgw_public
              #   az: us-east-1b
              #   cidr: 10.0.31.0/26
              #   route: rt_natgw
              #   public_ip: true
          - name: vpc_use1_az
            block: 10.1.0.0/19
            provider: azure
            region: us-east-1
            subnets:
              - name: net-nodes-public-1
                cidr: 10.1.0.0/24
              - name: net-nodes-private-1
                cidr: 10.1.1.0/24
              - name: net-nodes-public-2
                cidr: 10.1.2.0/24
              - name: net-nodes-private-2
                cidr: 10.1.3.0/24
              - name: net-nzdes-public-3
                cidr: 10.1.4.0/24
              - name: net-nodes-private-3
                cidr: 10.1.5.0/24
              - name: net-nodes-public-4
                cidr: 10.1.6.0/24
              - name: net-nodes-private-4
                cidr: 10.1.7.0/24
              - name: net-elb-public-1
                cidr: 10.1.10.0/24
              - name: net-elb-public-2
                cidr: 10.1.11.0/24
              - name: net-elb-public-3
                cidr: 10.1.12.0/24
              - name: net-elb-public-4
                cidr: 10.1.13.0/24
              - name: net-vpn-public-1
                cidr: 10.1.31.0/24

        roles:
           - { role: cloud-iam.mtulio }


License
-------

GPLv3

Author Information
------------------

[Marco Tulio R Braga](https://github.com/mtulio)
