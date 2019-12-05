Preparations
============

YAWIK needs PHP >=7.2 for execution and the described extensions from the :ref:`requirements<requirements>`.

For the installation via Composer (this is the easiest way at the moment) npm is needed. The Nodes Package Manager 
executes grunt tasks at the end of the installation which copy images, convert LESS to CSS and compress JS. 

Data is stored in a MongoDB. The easiest way is to install a MongoDB locally. If this is not possible, you can use 
a MongoDB provider like `mlab.com`_, ScaleGrid_ or google_.

Apache or nginx can be used as webserver. For testing you can use the PHP buildin server. 

And of course you need composer. 

In the different Linux distributions there are dirverse differences. So you have to proceed differently until an 
installation via composer works.

.. _mlab.com: https://mlab.com/
.. _ScaleGrid: https://scalegrid.io/
.. _google: https://console.cloud.google.com/launcher?q=mongodb


.. toctree::
   :maxdepth: 2

   preparations/ubuntu18.04   
   preparations/debian10
   preparations/mongo


