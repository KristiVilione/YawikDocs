.. index:: Import

.. _import:

SimpleImport
------------

.. sidebar:: SimpleImport

   =======================  ==========================================
   ``Repository``            `yawik/SimpleImport`_
   ``coverage``              |coverage|
   ``buid``                  |build|
   =======================  ==========================================

.. |coverage| raw:: html

	<a href='https://coveralls.io/github/yawik/SimpleImport?branch=master'><img src='https://coveralls.io/repos/github/yawik/SimpleImport/badge.svg?branch=master' alt='Coverage Status' /></a>


.. |build| raw:: html

        <a href="https://travis-ci.org/yawik/SimpleImport"><img src="https://travis-ci.org/yawik/SimpleImport.svg?branch=master"></a>



.. _yawik/SimpleImport: https://github.com/yawik/SimpleImport.git


The SimpleImport module offers a command line tool to import joboffers from feeds using a fix defined structure. It's usefull, if you plan to use 
YAWIK as a jobportal and if you would like to import jobs from customers.

The CLI offers the following functionalisites.


.. code-block:: sh

  Simple import operations
        
        yawik simpleimport import [--limit] [--name] [--id]    Executes a data import for all registered crawlers                                                 
        yawik simpleimport add-crawler                         Adds a new import crawler                                                                          

        --limit=INT                 Number of crawlers to check per run. Default 3. 0 means no limit                                                                                    
        --name=STRING               The name of a crawler                                                                                                                               
        --id=STRING                 The Mongo object id of a crawler                                                                                                                    
        --organization==STRING      The ID of an organization                                                                                                                           
        --feed-uri=STRING           The URI pointing to a data to import                                                                                                                
        --runDelay=INT              The number of minutes the next import run will be proceeded again                                                                                   
        --type=STRING               The type of an import (e.g. job)                                                                                                                    
        --jobInitialState=STRING    The initial state of an imported job                                                                                                                
        --jobRecoverState=STRING    The state a job gets, if it was deleted, but found again in later runs.                                                                             


        yawik simpleimport info    Displays a list of all available crawlers.
        yawik simpleimport info [--id] <name>    Shows information for a crawler
        yawik simpleimport update-crawler [--id] <name> [--rename] [--limit] [--organization] [--feed-uri] [--runDelay] [--type] [--jobInitalState] [--jobRecoverState]    Updates configuration for a crawler. 
        yawik simpleimport delete-crawler [--id] <name>    Deletes an import crawler

        <name>             The name of the crawler to delete.                                                                                                                           
        --id               Treat <name> as the MongoID of the crawler                                                                                                                   
        --rename=STRING    Set a new name for the crawler.                                                                                                                              

        yawik simpleimport guess-language [--limit]    Find jobs without language set and pushes a guess-language job into the queue for each.                    

        --limit=INT    Maximum number of jobs to fetch. 0 means fetch all.                                                                                                              


        yawik simpleimport check-classifications <root> <categories> [<query>]    Check job classifications.                                                      

        <root>          Root category ("industrues", "professions" or "employmentTypes")                                                                                              
        <categories>    Required categories, comma separated. E.g. "Fulltime, Internship"                                                                                             
        <query>         Search query for selecting jobs.                                                                                                                              


        --force    Do not ignore already checked jobs.




Feeds have to be formatted as defined in :ref:`the scrapy docs<scrapy:format>`.


Example: Add a feed to a an organization

You need an organizationId in order to add a crawler job. So at first you have to create a company in your yawik. The organizationId 
appears in an URL, if you try to edit the organization. 

.. code-block:: sh

    root@yawik:/var/www/YAWIK# ./vendor/bin/yawik simpleimport add-crawler --name=example-crawler \
                                                                      --organization=59e4b53e7bb2b553412f9be9 \
                                                                      --feed-uri=http://ftp.yawik.org/example.json
    A new crawler with the ID "59e4b5a87bb2b567468b4567" has been successfully added.


This command created a crawler in the mongo collection ``simpleimport.crawler`` and returns the ObjectId.


.. code-block:: sh

    > db.simpleimport.crawler.find({"_id":ObjectId("59e4b5a87bb2b567468b4567")}).pretty();
    {
            "_id" : ObjectId("59e4b5a87bb2b567468b4567"),
            "name" : "example-crawler",
            "organization" : ObjectId("59e4b53e7bb2b553412f9be9"),
            "type" : "job",
            "feedUri" : "http://ftp.yawik.org/example.json",
            "runDelay" : NumberLong(1440),
            "dateLastRun" : {
                    "date" : ISODate("1970-01-01T00:00:00Z"),
                    "tz" : "+00:00"
            },
            "options" : {
                    "initialState" : "active",
                    "_doctrine_class_name" : "SimpleImport\\Entity\\JobOptions"
            }
    }


.. note:: if you execute the command twice, the crawler will be added twice. If you want to remove a crawler, you have to
    do so on the mongo cli.
