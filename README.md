configurer
==========

Provides a way to simplify configuration management on Google App Engine applications (Java).

=========== Some basic notes =====================
A distributed configuration management for GAE application. Idea is to place configurations in cloud storage and poll for changes. Configurations are parsed and stored in Datastore and primarily served from Memcache. Configurations are further stored in in-mem cache per instance to avoid API call latencies. A flag is placed in an instance which is dirtied withconfiguration changes.

Configuration changes are polled via cron tasks and an additional task that gets kicked off with deployment changes.

There should a suppport for namespaces. Configuration changes per version should also be possible.


		Version (Default by default)   =========>  Namespace ========> Configs

In long term there should be a serving URL that should allow application admins to change configuration via UI and push them on separate versions.

As configuration files can be large in size, an streaming API should be used to read and parse configurations. N number of backups should be maintained and it should be easy to revert configurations if necessary.
