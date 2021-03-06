Trial Cookbook
==============

# Installation
## Requirements

* **Ruby >= 1.9.3**
* **Bundler**

### Clone repo:

    git clone https://github.com/freeminder/chef_cookbooks

### To install the dependencies, run bundle in chef_cookbooks/cookbooks/hb_trial:

    bundle

## Configuration
### ~/.chef/knife.rb should contain configuration:

    log_level                :info
    log_location             STDOUT
    node_name                'provisioner'
    client_key               '/home/path/.chef/provisioner.pem'
    validation_client_name   'validator'
    chef_server_url          'http://127.0.0.1:8889'
    knife[:chef_repo_path] = '/home/path/Documents/devel/chef_cookbooks'

    # AWS
    knife[:ssh_key_name] = 'id_rsa'
    knife[:aws_access_key_id] = 'xxxxxxxxxxxxxxxxxxxxxxxxx'
    knife[:aws_secret_access_key] = 'yyyyyyyyyyyyyyyyyyyyyyyyy'
    knife[:ssh_user] = 'ubuntu'

##### Set `knife[:ssh_key_name]` to your Amazon EC2 Key Pair name.
##### You can also refer to https://docs.chef.io/config_rb_knife_optional_settings.html for additional options.

##### Also replace 'id_rsa' with your Amazon EC Key Pair name in chef_cookbooks/cookbooks/hb_trial/recipes/aws_setup.rb.

### Generate client's key in ~/.chef:

    ssh-keygen -f provisioner.pem


### ~/.aws/config should contain configuration:

    [default]
    aws_access_key_id = xxxxxxxxxxxxxxxxxxxxxxxxx
    aws_secret_access_key = yyyyyyyyyyyyyyyyyyyyyyyyy
    region = eu-central-1

# Usage
### Run recipe in chef_cookbooks dir:

    chef-client -z cookbooks/hb_trial/recipes/aws_setup.rb


## Notes
Tested in 'eu-central-1' region only. AWS AMI can be different in another regions.
To set your own vhosts, edit [cookbooks/hb_trial/attributes/default.rb](attributes/default.rb).

The recipe doesn't deploy a NAT instance and route MySQL instance's traffic throught it since there is an issues with EIP:
https://github.com/chef/chef-provisioning-aws/issues/366

## License

Please refer to [LICENSE](/LICENSE).
