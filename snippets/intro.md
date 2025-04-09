
::: {.cell .markdown}

# Persistent storage on Chameleon

In this tutorial, we will practice using two types of persistent storage options on Chameleon:

* object storage, which you may use to e.g. store large training data sets
* and block storage, which you may use for persistent storage for services that run on VM instances (e.g. MLFlow, Prometheus, etc.)

To run this experiment, you should have already created an account on Chameleon, and become part of a project. You should also have added your SSH key to the KVM@TACC site and the CHI@TACC site.

:::

::: {.cell .markdown}

## Experiment resources 

For this experiment, we will provision 

* one block storage volume on KVM@TACC
* one object storage bucket on CHI@TACC
* one virtual machine on KVM@TACC, with a floating IP, to practice using the persistent storage

:::

::: {.cell .markdown}

## Open this experiment on Trovi


When you are ready to begin, you will continue with the next step! To begin this step, open this experiment on Trovi:

* Use this link: [Persistent storage on Chameleon](https://chameleoncloud.org/experiment/share/a1c68238-81f8-498d-8323-9d6c46cb0a78) on Trovi
* Then, click "Launch on Chameleon". This will start a new Jupyter server for you, with the experiment materials already in it.

You will see several notebooks inside the "data-persist-chi" directory - look for the one titled `0_intro.ipynb`. Open this notebook and continue there.

:::