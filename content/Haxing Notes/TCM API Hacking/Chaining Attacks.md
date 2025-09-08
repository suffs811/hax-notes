##### Bugs sometimes only have impact when they are combined with other attacks

For example, if you have a mass assignment vuln and you notice the value is a command/part of a command that gets executed in the backend and you have SSRF to make internal requests, you can change the parameter's value to a command injection payload (`; ls -la #`) and then hit the internal endpoint using SSRF to trigger the execution of the command.

