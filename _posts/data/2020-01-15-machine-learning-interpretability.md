---
title:  "An Overview of Interpretable Machine Learning"
date:   2020-01-15
categories: [data]
tags: [data, machine learning, statistics, interpretable ml]
---
Definition of interpretability ([1], [2])
Though there is little consensus on a formal definition of  interpretability in machine learning, one good definition is that interpretability is the ability to explain or to present in understandable terms  to a human.

Why interpretability?
Model interpretability is used to confirm below desiderata of ML systems:
Fairness: protected groups are not discriminated against
Privacy: the method protects sensitive information in the data
Reliability: whether algorithms is robust under parameter or input variation
Causality: the predicted change in output due to a perturbation will occur in real system
Trust: confidence from human users

-Regulations like GDPR and CCPA require algorithms that make decisions based on user-level predictions, which “significantly affect” users to provide explanation (“right to explanation”).

Scope of interpretation
Algorithm transparency: how does the algorithm create the model (focus on the knowledge of the algorithm, not the data or learned model)
Global model interpretability: comprehend the model at once, difficult to achieve due to the limitation of humans (imagine visualizing  a 5-dimensional hyperplane)
Interpret model prediction on a single instance

Properties of good explanations
Contrastive:
Selective:
Social:
Focus on abnormal:
Truthful:
Consistent with prior beliefs:
General and portable:

Evaluation Methods ([2]):
Application level evaluation: real humans, real tasks.
Human level evaluation: real humans, simplified tasks
Function level evaluation: no humans, proxy tasks

Taxonomy of interpretation methods:
Feature summary statistics, feature summary visualization, model internals, data point, intrinsically interpretable model

Interpretable Models
Linear models:
Easy to interpret when the number of features is limited (feature selection methods like LASSO can be used to restrict the number of features)
Interpretation:
Increasing the numerical feature by one unit changes the estimated outcome by its weight (numerical feature)
Changing the feature from the reference category to the other category changes the estimated outcome by the feature’s weight (categorical feature)
The importance of a feature in a linear regression model can be measured by the absolute value of its t-statistic
Visualization methods include weight plot, effect plot
For logistic regression, increasing the numerical feature by one unit changes the logarithm of the odds by its weight
Similar interpretations apply to other GLM or GAM models
Pros: universal, well-studied and accepted, backed up with statistical theories (confidence intervals, significant tests etc.)
Cons: assumptions on data generation process may not be valid, model performance sometimes often less than optimal, number of ways to transform model and features could be overwhelming

Decision trees:
Simple interpretation: starting from the root node of the tree, traverse till the leaf node, obtain a path connecting conditions explaining why the prediction was made
The overall importance of a feature is the summed reduction of Gini index
Pros: capturing interactions between features, has a natural visualization, create human-friendly explanations, no need to transform features
Cons: fails to deal with linear relationships, which goes hand in hand with the lack of smoothness; unstable (a few changes in the training dataset can create a completely different tree); only interpretable when they are short (the number of terminal nodes increases quickly with depth)

RuleFit ([7]):
Learns sparse linear models which include automatically detected interaction effects in the form of decision rules
First train an ensemble of trees (e.g random forest) based on the task of predicting the outcome of interest; Discard the prediction result but keep the decision rules decomposed from the trained trees, and train a sparse linear regression model (e.g LASSO)  using the original features and the additional features from the decision trees
Pros: automatically adds feature interaction to the linear models, good interpretable when rules are not too complicated (e.g maximum depth of the trees less than 3)
Cons: interpretability degrades with the increasing number of features, the performance of the model may not be optimal

Model-agnostic Methods
Flexible, same method can be used for any type of underlying models.

Partial Dependence Plot (PDP)
A global method: The method considers all instances and gives a statement about the global relationship of a feature with the predicted outcome.
Shows the marginal effect one or two features have on the predicted outcome of an ML model, estimated by calculating averages in the training data
Assumes no correlation between the feature of interest and the rest of the features
Pros: intuitive, clear interpretation, easy to implement, has a causal interpretation
Cons: maximum number of features in a PDP is two (due to human perception limitation), assumption of independence can be problematic, not revealing heterogeneous effects

Individual Conditional Expectation (ICE)
One line per instance that shows how the prediction for the instance changes when a feature changes
PDP is the average of the lines in ICE; the two can be combined together
Pros: even more understandable than PDP, can uncover heterogeneous effects
Cons: same cons as PDP, plus can be overcrowded

Accumulated Local Effects (ALE) ([9])
A fast and unbiased alternative to PDP, which describes how features influence the predictions of an ML model on average
Addresses the feature interaction problem with PDP and ICE
Calculates the differences in predictions instead of averages based on the conditional distribution of the features (instead of the marginal distribution in PDP and ICE)
ALE is preferred over PDP as a rule of thumb
Pros: unbiased (still works when features are correlated), faster to compute than PDPs, clear interpretation
Cons: can be a bit shaky based on the selection of intervals, not accompanied by ICE

Feature Interaction ([7])
When features interact, the prediction cannot be expressed as the sum of the feature effect. One way to estimate the interaction strength is through Friedman’s H-statistic, which is the difference between the observed partial dependence function and the decomposed one without interactions. Friedman and Popescu also proposed a test statistic to evaluate whether the H-statistic differs significantly from zero.
Pros: backed up by underlying theory, possible to analyze higher order interactions, dimensionless and between 0 and 1
Cons: computationally expensive, the statistic is not yet available in a model-agnostic version

Feature Importance
Permutation importance: increase in the prediction error after we permute the feature’s values
A feature is ‘important’ if shuffling its values increases the model error, because in this case the model relied on the feature for the prediction
When two features interact with each other, the importance of the interaction is included in the importance measurement of both features, the sum is larger than the drop in performance.
Not clear whether should compute importance on training or test data
Pros: easy interpretation, highly compressed, global insight, does not require retraining the model
Cons: unclear whether compute on training or test data, need access to the true outcome, depends on shuffling the feature which adds randomness, biased when features interact, adding a correlated feature can decrease the importance of the associated feature by splitting the importance between both features

Global Surrogate
An interpretable model (linear model, decision tree etc.) trained to approximate the predictions of a black box model. The labels are the output of the black box model instead of the true labels.
Not too useful. There’s a trade-off between approximation accuracy and interpretability of the surrogate model.
Pros: flexible, intuitive, easy to measure performance by R-square
Cons: no clear cut for R-square, could close to the black box model on one dataset but diverge on another

Local Surrogate (LIME) ([10])
Interpretable models that are used to explain individual predictions of black box ML models
Local interpretable model-agnostic explanations (LIME) generates a new dataset consisting of perturbed samples and the corresponding predictions of the black box model. On this new dataset LIME then trains an interpretable model, which is weighted by the proximity of the sampled instances to the instance of interest.
The learned model should be a good approximation of the machine learning model predictions locally, but it does not have to be a good global approximation.
Different methods are used to generate  the perturbed samples for different data formats: superpixels for images, randomly removing words for texts etc.
Pros: explanations are short and contrastive, works for tabular data, images, and texts, have a good fidelity measure
Cons: a correct definition of neighbors remains unresolved for tabular data, current sampling method ignores feature interaction, instability
Note: the method is promising but is still in development, where research opportunities lie

Integrated Gradients ([4])
Integration of the gradients of predictions with respect to features, along the linear interpolation path between the instance of interest and a baseline instance.
Satisfies two desirable characteristics (axioms) for feature attribution methods: sensitivity and implementation invariance; also proved that previous methods DeepLift, Layer-wise relevance propagation (LRP), de-convolutional network, and guided-back-propagation violated one axiom or the other.
Fast and scalable, only requires a few calls to the gradient operator (compared to the Shapley Values)

Shapley Values
A method from coalitional game theory, which describes how to fairly distribution the prediction among the features
The Shapley value is the average marginal distribution of a feature value across all possible coalitions, which is a computation of feature contributions for single predictions for any machine learning model
The only attribution method that satisfies efficiency, symmetry, dummy, and additivity, which together can be the definition of a fair payout
Shapley values might be the only method to deliver a full explanation. In situations where law requires explainability - like GDPR’s ‘right to explanations’,  the Shapley values might be the only legally compliant method, since it’s based on a solid theory, and distributes the effects fairly
Very expensive to compute, usually through Monte-Carlo sampling
Pros: the difference between the prediction and the average prediction is fairly distributed among the features of the instance, allows contrastive explanation, backed up by a solid theory
Cons: computationally expensive, sampling may increase variance, use all features thus not sparse, no prediction model like LIME (where you can make hypothetical statements), problematic when features are correlated as sampling from the feature’s marginal distribution
Note: open research problem as of how to sample when features are correlated

SHAP

Example-based Explanations

Counterfactual Explanations
Adversarial Examples
Prototypes and Criticisms
Influential Instances


Resources:
[1] Interpretable Machine Learning Book (link)
[2] Towards a rigorous science of interpretable machine learning (link)
[3] Explaining explainability (link)
[4] Interpretable Machine Learning: the fuss, the concrete and the questions (link)  
[5] Axiomatic Attribution for deep networks (link)
[6]A unified approach to interpreting model predictions (link)
[7] Predictive learning via rule ensembles (link)
[8] Captum: a model interpretability and understanding library for PyTorch (link)
[9] Visualizing the Effects of Predictor Variables in Black Box Supervised Learning Models (link)
[10] Why should I trust you?: Explaining the predictions of any classifier (link) 		
