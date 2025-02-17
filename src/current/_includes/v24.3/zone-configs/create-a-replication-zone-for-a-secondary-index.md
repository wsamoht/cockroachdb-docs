{{site.data.alerts.callout_success}}
The [Cost-based Optimizer]({% link {{ page.version.version }}/cost-based-optimizer.md %}) can take advantage of replication zones for secondary indexes when optimizing queries.
{{site.data.alerts.end}}



The [secondary indexes]({% link {{ page.version.version }}/indexes.md %}) on a table will automatically use the replication zone for the table. You can also add distinct replication zones for secondary indexes.

To control replication for a specific secondary index, use the `ALTER INDEX ... CONFIGURE ZONE` statement to define the relevant values (other values will be inherited from the parent zone).

{{site.data.alerts.callout_success}}
To get the name of a secondary index, which you need for the `CONFIGURE ZONE` statement, use the [`SHOW INDEX`]({% link {{ page.version.version }}/show-index.md %}) or [`SHOW CREATE TABLE`]({% link {{ page.version.version }}/show-create.md %}) statements.
{{site.data.alerts.end}}

{% include_cached copy-clipboard.html %}
~~~ sql
> ALTER INDEX vehicles@vehicles_auto_index_fk_city_ref_users CONFIGURE ZONE USING num_replicas = 5, gc.ttlseconds = 100000;
~~~

~~~
CONFIGURE ZONE 1
~~~

{% include_cached copy-clipboard.html %}
~~~ sql
> SHOW ZONE CONFIGURATION FROM INDEX vehicles@vehicles_auto_index_fk_city_ref_users;
~~~

~~~
                         target                        |                                 raw_config_sql
+------------------------------------------------------+---------------------------------------------------------------------------------+
  INDEX vehicles@vehicles_auto_index_fk_city_ref_users | ALTER INDEX vehicles@vehicles_auto_index_fk_city_ref_users CONFIGURE ZONE USING
                                                       |     range_min_bytes = 134217728,
                                                       |     range_max_bytes = 536870912,
                                                       |     gc.ttlseconds = 100000,
                                                       |     num_replicas = 5,
                                                       |     constraints = '[]',
                                                       |     lease_preferences = '[]'
(1 row)
~~~
