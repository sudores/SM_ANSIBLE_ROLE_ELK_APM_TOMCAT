ELK APM INSTALLATION TOMCAT
=========

This role should be used to install elastic apm java agent to tomcat servers. 
Also could be used to uninstall agent installed with it

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
```yaml
instrumentations: see default in defaults       - select elastic apm [instumentation](https://www.elastic.co/guide/en/apm/agent/java/current/config-core.html#config-enable-instrumentations)
log_level: ERROR                                - log level for elastic apm
apm_server_url: http://sm-mon.smiddle.lab:8200  - apm server url   
catalina_home: /opt/tomcat/                     - catalina home  path from root
enable_profiler: false                          - enable profiling capabilities
apm_args_templete: |                            - template for setenv.sh block 
  export CATALINA_OPTS="$CATALINA_OPTS -javaagent:{{ catalina_home }}/bin/apm-agent.jar"    
  export CATALINA_OPTS="$CATALINA_OPTS -Delastic.apm.service_name=${HOSTNAME}"    
  export CATALINA_OPTS="$CATALINA_OPTS -Delastic.apm.server_url={{ apm_server_url }}"    
  export CATALINA_OPTS="$CATALINA_OPTS -Delastic.apm.log_level={{ log_level }}"    
  export CATALINA_OPTS="$CATALINA_OPTS -Delastic.apm.application_packages={{ java_packages.stdout_lines[0] }}"    
  export CATALINA_OPTS="$CATALINA_OPTS -Delastic.apm.environment={{ host_env }}"    
  export CATALINA_OPTS="$CATALINA_OPTS -Delastic.apm.profiling_inferred_spans_enabled={{ enable_profiler }}"    
  export CATALINA_OPTS="$CATALINA_OPTS -Denable_instrumentations={{ instrumentations }}"    
```
Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      vars: 
        - catalina_home: /tomcat/
        - enable_profiler: true
        - instrumentations: spring-amqp,anotation
      roles:
         - role: gorobchenkoa.apm_elastic

License
-------

BSD

Author Information
------------------

Author: HorobchenkoA
Company: Smiddle LTD

