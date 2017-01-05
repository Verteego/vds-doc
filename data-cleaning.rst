#############
Data cleaning
#############

.. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/data-cleaning.png

The data cleaning module provides a powerful user interface to define a stack of data cleaning tasks that can be applied to your dataflow.

**Technology**

The data cleaning module is powered by Google's Open Refine Technology. You can find a full user documentation `here <https://github.com/OpenRefine/OpenRefine/wiki>`_

**General Refine Expression Language (GREL)**

The data cleaning module integrates a powerful expression language allowing to apply precise transformation and cleaning tasks to your data.

`GREL documentation <https://github.com/OpenRefine/OpenRefine/wiki/General-Refine-Expression-Language>`_

**Running cleaning tasks in dataflow**

The data flow module offers several processors that can be used to apply a cleaning script to a data flow. You can find them amongst the other processors by typing "Openrefine" in the processor search field.

Currently there are processors for XLS, XLSX, CSV and TSV support.