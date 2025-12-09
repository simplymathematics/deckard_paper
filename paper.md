---
title: 'deckard: A Declarative Tool for Machine Learning Robustness Evaluations'
tags:
  - Python
  - machine learning
  - adversarial AI
  - adversarial ML
  - artificial intelligence
  - robustness
authors:
  - name: Charles Meyers
    orcid: 0000-0002-1277-9811
    affiliation: 1
affiliations:
  - name: Department of Computing Science, Ume\aa{} University, Ume\aa{}
    index: 1
    ror: 05kb8h459

date: 15 April 2025
bibliography: bibliography.bib
output:
 pdf_document:
 keep_tex: true
---

# Summary 


The software package presented, called `deckard`, is a modular toolkit that streamlines and standardizes machine learning (ML) experimentation, with a particular emphasis on evaluating models under worst-case perturbations[@fgm]. 
It offers a flexible, extensible framework for defining, running, and analysing end-to-end ML pipelines in settings where inputs may be manipulated by an attacker or corrupted by unexpected noise [@biggio_evasion_2013].

Built on the Hydra configuration system [@hydra], `deckard` supports declarative YAML-based configuration for data preprocessing, model training, and adversarial attack pipelines. 
This makes it easy to run reproducible, framework-agnostic experiments across a wide range of ML domains.

Beyond configuration management, `deckard` provides tools for distributed and parallel execution, automated hyperparameter optimization, visualization, and result aggregation. 
By handling much of the engineering overhead common in adversarial ML research, it lets researchers focus on developing ideas rather than juggling infrastructure. 
The system also ensures rigorous benchmarking by keeping an auditable record of configurations, seeds, and intermediate outputs throughout each experiment.

Compatible with multiple ML frameworks and a variety of attack methods, `deckard` works both as a backend for large-scale automated testing and as a platform for detailed empirical analysis. 
With its unified interface for experiment control, the toolkit accelerates the development of robust models and helps bridge the gap between exploratory research and reproducible, trustworthy results .

To avoid any ambiguity about contributions, we clarify that `deckard` is a method and software system introduced in this work. 
Although the implementation is publicly available on GitHub, the modular design, configuration-driven framework, and experimental methodology are original to this paper. 
While the prior works outlined in the Usage Section can be fully reproduced with this software, this paper is the first to present the system as a cohesive method, fully specified, documented, and intended for broad reuse.


# Statement of need

The software package presented, called `deckard`, is a modular toolkit that streamlines and standardizes machine learning (ML) experimentation, with a particular emphasis on evaluating models under worst-case perturbations [@fgm]. 
It offers a flexible, extensible framework for defining, running, and analysing end-to-end ML pipelines in settings where inputs may be manipulated by an attacker or corrupted by unexpected noise [@biggio_evasion_2013].

Built on the Hydra configuration system [@hydra], `deckard` supports declarative YAML-based configuration for data preprocessing, model training, and adversarial attack pipelines. 
This makes it easy to run reproducible, framework-agnostic experiments across a wide range of ML domains.

Beyond configuration management, `deckard` provides tools for distributed and parallel execution, automated hyperparameter optimization, visualization, and result aggregation. 
By handling much of the engineering overhead common in adversarial ML research, it lets researchers focus on developing ideas rather than juggling infrastructure. 
The system also ensures rigorous benchmarking by keeping an auditable record of configurations, seeds, and intermediate outputs throughout each experiment.

Compatible with multiple ML frameworks and a variety of attack methods, `deckard` works both as a backend for large-scale automated testing and as a platform for detailed empirical analysis. 
With its unified interface for experiment control, the toolkit accelerates the development of robust models and helps bridge the gap between exploratory research and reproducible, trustworthy results .

To avoid any ambiguity about contributions, we clarify that `deckard` is a method and software system introduced in this work. 
Although the implementation is publicly available on GitHub, the modular design, configuration-driven framework, and experimental methodology are original to this paper. 
While the prior works outlined in the Usage Section can be fully reproduced with this software, this paper is the first to present the system as a cohesive method, fully specified, documented, and intended for broad reuse.


# Usage
Various versions of this software have been used in several recently published and not-yet-published works by the author of this paper, all of which are available in the `examples` folder in the source code repository [https://github.com/simplymathematics/deckard](https://github.com/simplymathematics/deckard).
One published work, now reproducible via the `examples/attack_defence_survey` folder, includes a large survey of attacks and defences against canonical datasets and models [@meyers2023safety].
Another work analysed the run-time requirements of attacks against a particular model before and after retraining against those attacks [@meyers2024massively] (reproducible via `examples/retraining`). 
The next paper formalised a method for estimating the time-to-failure of a given model against a suite of attacks and introduce a metric that quantifies the ratio of attack and training cost [@meyers_aft] (reproducible via `examples/survival_heuristic`).
Furthemore, a not yet published work uses this time-to-failure model as a mechanism for analysing the cost efficacy of various hardware choices in the context of adversarial attacks (reproducible via `examples/power`) [@trashfire].
Another work exploits the tooling to train a custom model that is designed to run client-side by using compression algorithms to measure the distance between text (reproducible via `examples/compression`).


# Experiment Management
Typically ML projects are composed of long and complex pipelines that are highly dependent on a number of parameters that must be configured by either the model builder or attacker. 
Due to the large scale and cost associated with training ML models, it is often necessary to tune a model using many indivudal model configurations (often called _hyper-parameters_).
To determine adversarial robustness, one of many benchmark datasets is first sampled, then preprocessed, sent to a model, with optional pre- and post-processing defences, and then scored according to some chosen metric which may include the performance against any number of adversarial attacks.
Each stage in this example pipeline might include tens or hundreds of possible sets of hyper-parameters that must be exhaustively tested.
Furthermore, this problem scales drastically as we include more and more stages in a pipeline since each additional stage introduces a new combinatorial layer of complexity, rapidly expanding the total number of potential configurations that must be evaluated for robustness and be reproducible for posterity.
Not only does `deckard` provide a standard way to document and configure these hyper-parameters, it gives each experiment an auditable identifier.

# Reproducibility and Auditability

For ML, various regulatory and legal frameworks govern safety [@ai_eu_act;@iso26262;@IEC61508;@IEC62034], privacy [@ai_eu_act;@gdpr;@hipaa;@coppa] and/or transparency [@ai_eu_act;@ai_pipeline_regulation].
The software package presented here provides a machine- and human-readable format for creating reproducible and auditable experiments as required by various regulations.
In addition, several examples connected to both published and not-yet-published work live in the `examples` folder in the repository, allowing for easy reproducibility of several extensive sets of experiments across several popular ML software frameworks.
The `power` example provides a reproducible way to run a suite of adversarial tests using popular cloud-based platforms and the `retraining` and `survival_heuristic` examples provide examples of both CPU and GPU-based parallelisation, respectively. 

The `basics` subfolder provides a minimum working example for each of the supported ML frameworks: `tensorflow` [@tensorflow], `pytorch` [@pytorch], `scikit-learn` [@sklearn], and `keras` [@keras]. 
The basics folder also provides examples of various classes of adversarial examples:
_poisoning_ attacks that change model behaviour by injecting data during training [@biggio2012poisoning], _inference_ attacks [@li2021membership] that attempt to reverse engineer properties of the training data, _extraction_ attacks that attempt to reverse engineer the model [@extraction_attack], and _evasion_ attacks that induce errors of classification during run-time [@meyers2023safety].
The parameters file for each experiment ensures that a given pipeline can be reproduced and the standardised format allows us to derive a hash value that is hard to forge but easy to verify. 
Not only does this hash serve as an identifier to track the state of an experiment, but also serves as a way to audit the parameters file for tampering. 
Likewise, by using `dvc` [@dvc] to track any input or output files specified in the parameters file, the software associates each score file with a identifier that is easy to track and verify, but hard to forge---ensuring that forged or modified results are easy to spot in version-controlled experiment repository. 

# Parallel and Distributed Design

Since ML projects can exploit specialized hardware such as multi-core processors or GPUs, and often rely on clusters of machines for large-scale data processing, it was necessary to enable parallel and distributed experiment execution and model optimization. By leveraging the `hydra` configuration framework, `deckard` automatically supports optimization libraries like `nevergrad` [@nevergrad], `Adaptive Experimentation` [@ax], and `optuna` [@optuna], making the software modular and extensible. Additionally, experiments can be managed using a variety of popular job schedulers, including `Ray` [@ray], `Redis Queue` [@rq], and `slurm` [@slurm] for distributd jobs or `joblib` [@joblib] for jobs on a single machine.

By using a declarative design, a given set of experiments can be specified once and executed seamlessly across different backends without modification to the underlying codebase.
This makes `deckard` both adaptable and scalable, suitable for use on personal laptops, multi-node servers, or large-scale, high-performance clusters. 
When configured appropriately, experiment batches can be parallelized, enabling massive parameter sweeps, ensemble evaluations, or adversarial robustness tests to be executed in parallel---reducing turnaround time while maintaining strong guarantees on reproducibility and auditability. 
The design of the presented software prioritizes clarity and maintainability by capturing each experimental configuration as a YAML artifact, making both successful and failed runs equally traceable and shareable. 
This approach transforms experiment tracking from an afterthought into a first-class component of the trustworthy ML workflow.


# Funding

Financial support has been provided in part by the Knut and Alice Wallenberg Foundation grant number 2019.0352 and by the eSSENCE Programme under the Swedish Government's Strategic Research Initiative.

# Acknowledgements

The author would like to thank Aaron MacSween, Abel Souza, and Mohammad Saledghpour Reza for
their guidance in software design principles. 
In particular, the author appreciates Mohammad's code and documentation regarding cloud-based and other Kubernetes deployments.
The author would like to thanks his advisors, Erik Elmroth and Tommy Löfstedt for their research expertise, funding, and patience.

# References
