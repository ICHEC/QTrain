# Scynergy 2025

``````{admonition} Welcome to SCynergy 2025 workshop!
:class: information

**advancing Supercomputing, AI, and quantum technologies in Europe**

Supercomputing, artificial intelligence, and quantum computing are reshaping industries, research, and society at large. SCynergy 2025 brings together businesses, researchers, and policymakers to explore practical applications, share knowledge, and build collaborations that will shape the future of these technologies in Europe. 

Organised by Supercomputing Luxembourg, with the support of EuroHPC JU and Women in HPC, SCynergy provides a platform for experts and decision-makers to discuss how high-performance computing (HPC), AI, and quantum technologies can be applied effectively across sectors such as finance, space, healthtech, and industry. 

```{card} See the Official program at the website:
- [https://events.eurocc.lu/scynergy-2025](https://events.eurocc.lu/scynergy-2025)

```
``````


## Quantum Computing in action

```{card}
This page lists the necessary content for the session **Quantum Computing in Action** of the SCynergy 2025 workshop.

The session covers the fundamentals of Quantum Computing and practical applications. It’s ideal for those looking to understand the current state and future potential of quantum technologies.

```

---

## Workshop summary
Following is the schedule of the session on [April 28, 2025](https://events.eurocc.lu/scynergy-2025/content/trainings).

```{table}
:align: center
:widths: grid
|Time          |Content |
|---           |---     |
|14:40 – 14:50 | Outline of the training session|
|14:50 – 15:35 |Quantum Computing refresh (setup, tutorial, interactive, discussion oriented)|
|15:35 – 15:40 |Outline of exercises (walk-through & hands-on)|
|15:40 – 16:20 |Exercise 1 – Quantum Chemistry                |
|16:20 – 16:50 |Break                                         |
|16:50 – 17:30 |Exercise 2 – QSVM4EO                          |
|17:30 – 18:10 |Exercise 3 – QUBO/QAOA                        |
|18:10 – 18:20 |Wrap-up                                       |
```

---

## The workshop 

``````{card}
The workshop tries to familiarise the participants with quantum computing through simple examples from three areas.
1. [Quantum chemistry: study of a molecule](./exercise1.md)
2. [Quantum machine learning for earth observation](./exercise2.md)
3. [Quantum optimization](./exercise3.md)

We will go through each of these during the session, where -

```{mermaid}
:align: center

flowchart TD
    A(We describe the problem)
    B(We see how this problem maps to Quantum computing)
    C(We see how the Quantum computing problem is solved)
    A --> B --> C
```
We have a list of [prerequisites](./prerequisite.md), circulated before for every participant. However they aren't mandatory for the session, merely listed incase one needs to refresh some basics.
``````

```{admonition} Computing aspects

- All the content, including this page and the tutorial notebooks, is hosted in a public github repository
    - https://github.com/ICHEC/QTrain
    - It should be downloaded using 
    
    `git clone https://github.com/ICHEC/QTrain.git`

- All training material uses [Qiskit](https://www.ibm.com/quantum/qiskit) package.
- The primary portal for training is jupyterlab server jlab.lxp.lu Hosted by LuxProvide.

```

```{admonition} Steps on jlab
:class: information

After logging into jlab.lxp.lu, we need to load the following modules in order -

1. `Qiskit/1.4.1-foss-2024a`
2. `matplotlib/3.9.2-gfbf-2024a`
3. `Seaborn/0.13.2-gfbf-2024a`

You should open any notebooks/terminals **after** loading the above modules on jlab.
```


```{warning}
closing jupyter lab does not stop the job on the HPC node. One needs to release the allocation
later using `scancel` once the session is over.
```

## Acknowledgements

We extend our gratitude to the Irish Centre for High-End Computing (ICHEC) and University of Galway for providing computing and for all-encompassing invaluable support. This project was funded by the **EuroHPC JU** under **grant agreement No 951732** and Ireland.

``````{card}
```{image} ./images/ICHEC.png
:width: 322px
```
```{image} ./images/UoG_.png
:width: 300px
```
```{image} ./images/EuroCC-Ireland.png
:width: 100px
```
``````