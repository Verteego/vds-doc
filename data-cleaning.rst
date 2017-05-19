#############
Data cleaning
#############

.. image:: https://vds-doc.readthedocs.io/en/latest/_static/images/data-cleaning.png

The data cleaning module provides a powerful user interface to define a stack of data cleaning tasks that can be applied to any file running through your dataflow.

**Technology**

The data cleaning module is powered by Google's Open Refine technology. You'll find a full user documentation `here <https://github.com/OpenRefine/OpenRefine/wiki>`_.

**General Refine Expression Language (GREL)**

The data cleaning module comes with a powerful expression language allowing to apply precise transformation and cleaning tasks to your data.

`Check out the GREL documentation <https://github.com/OpenRefine/OpenRefine/wiki/General-Refine-Expression-Language>`_

**Running cleaning tasks in dataflow**

The dataflow module offers special processors that can be used to apply a cleaning script to a data flow. You may find them amongst the other processors by typing *openrefine* in the processor search field.

Currently there are processors for XLS, XLSX, CSV and TSV support.