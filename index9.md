Come impostare INDEX 9 per la cancellazione effettiva dei file.


Modificare i file
```
ecmengine.war/classes/alfresco/extension/mt/mt-contentstore-context.xml
ecmengine.war/classes/alfresco/content-services-context.xml
```

Impostare 
```
<property name="protectDays" >
   <value>0</value>
</property>
```

Cosi facendo index imposterà i file eliminati come fisicamente eliminabili dopo 0 giorni

Nella cartella dei file di properties modificare
```
custom-scheduled-jobs-context.xml
```

aggiungendo 

```
<bean id="contentStoreCleanerTrigger" class="org.alfresco.util.CronTriggerBean">
    <property name="jobDetail">
        <ref bean="fileContentStoreCleanerJobDetail" />
    </property>
    <property name="scheduler">
        <ref bean="schedulerFactory" />
    </property>
    <property name="cronExpression">
        <value>0 0/5 * * * ?</value>
    </property>
</bean>
```

Cosi facendo ogni 5 minuti verranno controllati ed eliminati fisicamente i file
