# Proctor
Administers your tests.

************************

## The idea

Proctor aims to provide an easily extensible platform for running configuration management acceptance tests. Proctor itself is fully platform agnostic, and relies on platform providers to accomplish many of its core tasks like provisioning hosts, and running test steps like applying a configuration to a system under test. Proctor is written in PowerShell to be fully cross platform compatible. This means that your acceptance tests should run exactly the same whether run from Linux, Mac or Windows.

### Architecture

This module should provide a generic language for users to specify a set of hosts on which to run their acceptance tests, setup and teardown procedures, the test procedures to run, and verifying the results of those tests.

This module on it's own will provide very little of the details for how to accomplish those tasks. It will interact with providers to accomplish the tasks specified. It will instead specify a generic interface for most tasks which it will hand off to a provider which will be expected to know how to accomplish the requested tasks. 

For example, this module will provide a generic way to specify that you would like to run tests on a Linux master with a Windows node that will act as the actual system under test. The task of requesting or provisioning nodes from a back end system like VCenter, Azure, or VMPooler will be accomplished by a provisioning provider module. That module will simply be another PowerShell module designed to override the default stub implementations of functions that the framework module will call, and from which it expects to receive a result that represents the provisioned nodes.

Another provider module will allow the framework to communicate with the returned modules to accomplish the pre suite setup tasks.

The framework should then analyze the requested test pattern for order of operations in the style of RSpec to determine what tests need to be run and which `before` and `after` blocks should be run in which order during the test run.

Another provider module will allow the framework to communicate with platform under test. Examples would be Puppet, Chef, Ansible, etc. This provider should implement a generic testing language to the extent possible, but is allowed to implement whatever platform specific language is required so that tests make sense to users of the platform under test.

The framework should know how to receive the results of each test step from the provider and provide a language for asserting what the result should be.

The framework should then be capable of gathering results and visualizing them on the commandline and exporting them as parsable files.

### Possible provider types and examples

- Machine Provisioner
  - VCenter
  - Azure
  - AWS
  - VMPooler
  - Docker
  - Local
- Communication Protocol
  - WinRm
  - SSH
- Platform
  - Puppet
  - Chef
  - Ansible

* Note: It would be nice if it was capable of downloading and using a provider plugin directly from source control given a url and branch name. This way either testing, or just forking and customizing a given provider would be easier.

### What should this thing be able to do?

- Provision test hosts from a virtualization provider
- Pre suite setup including installation of the platform under test
- Test file analysis to determine order of operations
- Test step execution
- Validation of results
- Visualization of results on the commandline
- Export of test results as an artifact
