# Contributing

If you want to contribute to the StationMaster project, feel free!

## Including it in your Ansible playbook

You can  use an include statement from within a task:

```yaml
# Run StationMaster
- include: /private/var/www/stationmaster-ansible/tasks/main.yaml base={{ base }} devdomain=dev.company.com branch={{branch}}
```

This is useful for development as you can test your changes immediately.

# Submitting your changes

Simply push your changes to a branch on your repository and submit a pull request to the [canonical repo](https://github.com/radify/stationmaster).