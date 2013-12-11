# ansible-rails

Ansible extensions to work with bundler and rails related commands.

Also contains a role which deploys just like capistrano/ mina.

## example usage

``` yaml
---
- hosts: server
  user: app
  gather_facts: False
  vars:
    user: app
    home_directory: "/home/{{ user }}"
    rails_env: "staging"

  roles:
    -
      role: deploy

      repo: git@example.com:app
      branch: develop

      deploy_to: "{{ home_directory }}"
      build_path: "{{ deploy_to }}/releases/{{ timestamp.stdout }}"
      shared_path: "{{ deploy_to }}/shared"
      current_path: "{{ deploy_to }}/current"
      migrate: yes
      compile_assets: yes

      symlinks:
        - vendor/bundle
        - public/assets
        - log
        - .env
        - config/database.yml
```

## requirements

  - all gem binaries (e.g. bundle, rake, rails) need to be locateable using the $PATH. Make sure to setup properly

## important features:

  - it can be reused multiple times inside a single playbook for separate deployments.
  - works with rvm, rbenv or system ruby installations.
  - it's using a bare copy of the repository to deploy.
  - migration and asset compilation can be de-activated as needed.