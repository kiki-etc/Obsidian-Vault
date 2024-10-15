Deep learning is a subset of machine learning that focuses on training neural networks with many layers (deep networks) to model complex patterns in data. It’s inspired by the structure and functioning of the human brain, where neurons are interconnected and process information in layers.
<br>There are four key components:
1. The *data* that we can learn from.
2. The *model* of how to transform the data.
3. The *objective function* that quantifies how well the model is doing.
4. An *algorithm* to adjust the model's parameters to optimize the objective function.
## Data
Deep learning requires large amounts of data to produce models that are effective. In order to usefully work with said data, it must have some numerical representation.
In cases where every example (in the data) is characterized by the same number of numerical features, we say that the inputs are fixed-length vectors and we call the (constant) length of the vectors the *dimensionality* of the data.
## Models
Most machine learning involves transforming data, in some sense. Deep learning is differentiated from classical approaches principally by the set of powerful models that it focuses on. These models consist of many successive transformations of data that are chained together.
## Objective Functions
Objective functions help models improve over time by calculating the success. They are called *loss functions* when they calculate how badly the model is doing. We're trying to maximize the objective function but minimize the loss function.
## Optimization Algorithms
These are algorithms used to search for the best possible parameters for minimizing the loss function.
# Machine Learning Problems
## Supervised Learning
Supervised learning describes tasks where a model is trained on data that has input and labels and is asked to predict labels based on given input features. Each feature-label pair is called an *example*.
### Regression
A **regression problem** refers to a type of supervised learning task where the goal is to predict a continuous numerical value based on input features. The model learns the relationship between the input variables (independent variables) and the continuous target variable (dependent variable).
### Classification
Here, the goal is to predict a **discrete class label** for a given input. The model is trained on labeled data, where each input is associated with a specific category or class, and its job is to assign new, unseen inputs to one of the predefined classes.
Usually, the output is a set of probabilities that the unseen input will fall into either of the predefined classes. These probabilities can then be used to make a decision.
### Tagging
This is a type of **multi-label classification problem** where the goal is to assign multiple labels or “tags” to a single input. This is different from standard classification tasks (binary or multi-class), where each input is assigned only one label. In tagging problems, an input can belong to several categories simultaneously.
### Sequence Learning
**Sequence learning** uses data that consists of sequences, and the goal is to model the dependencies and relationships between elements in these sequences. The order of the data points is crucial, and the model is designed to capture patterns, correlations, or temporal relationships in the data.
Some examples include machine translation, text-to-speech, speech recognition, etc.
## Unsupervised and Self-Supervised Learning
**Unsupervised learning** is a type of machine learning where the model is trained on data without labeled outputs. In this approach, the algorithm attempts to identify patterns, structures, or relationships within the data without any prior knowledge of the categories or outcomes. This makes unsupervised learning particularly useful for exploratory data analysis and tasks where labeled data is scarce or unavailable.
For example, imagine a dataset containing customer transaction records without any labels. An unsupervised learning algorithm could analyze the data to identify clusters of customers who have similar purchasing habits, allowing businesses to tailor marketing strategies to different customer segments.
<br>Self-supervised learning lies between supervised and unsupervised learning. In self-supervised learning, the model is trained on data that does not have explicit labels but generates its own supervisory signal from the input data itself. This approach leverages the inherent structure of the data to create tasks that allow the model to learn meaningful representations.
### Interacting with an Environment
Here, we focus on building intelligent agents, i.e. active decision makers as opposed to predictive models. Traditional methods use *offline learning*, which has models learn from data in isolation from the context in which the data was retrieved. A dynamic environment poses new modeling questions about memory and the effect of the passage of time on the data.
### Reinforcement Learning
Reinforcement learning gives a very general statement of a problem in which an agent interacts with an environment over a series of time steps. At each time step, the agent receives some observation from the environment and must choose an action that is subsequently transmitted back to the environment (via what is sometimes called an *actuator*). After each loop, the agent is given some reward form the environment.

>[!note]- Attention Mechanisms
>An **attention mechanism** is a neural network component designed to enhance the performance of models, particularly in tasks involving sequential data, such as natural language processing (NLP) and computer vision. The main idea behind attention is to enable the model to focus on specific parts of the input data that are more relevant for making predictions or generating outputs, rather than treating all parts equally.
><br>**Key Concepts of Attention Mechanisms:**
>1. **Relevance**: Attention allows the model to weigh the importance of different input elements when making predictions. For example, in a sentence, some words may be more critical than others for understanding the meaning.
>2. **Contextualization**: The attention mechanism creates a context vector that summarizes the relevant information from the input, which can be dynamically adjusted based on the task or query at hand.
>3. **Dynamic Weights**: Attention mechanisms assign different weights to input elements based on their relevance to the current task. This allows the model to focus more on important information and ignore less relevant parts.

