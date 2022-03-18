# Modifications and Hyperparameter Tuning

Here we provide details about our way of tuning hyperparameters for each model.

## Adversarial Training
### Adv
- **Intro:**  
    Adversarial training employ an additional discriminator component, which shares the same encoder with the main model and is trained to identify protected attribute. In addition to make correct predictions, the main model is also trained to unlearn the discriminator.
- **Hyperparameters:**

    | Name                    | Default value | Description                                                              |
    |-------------------------|---------------|--------------------------------------------------------------------------|
    | adv_update_frequency    | Batch         | Epoch \| Batch                                                           |
    | adv_level               | last_hidden   | input \| last_hidden \| output                                           |
    | adv_lambda              | 1             | strength of adversarial regularization                                   |
    | adv_hidden_size         | 300           | number of hidden units of each hidden layer for the main task classifier |
    | adv_n_hidden            | 2             | number of hidden layers                                                  |
    | adv_dropout             | 0             | dropout probability                                                      |
    | adv_activation_function | ReLu          | nonlinear activation function for the main task model                    |
    | adv_batch_norm          | False         | apply 1d batch norm to the model                                         |

- **Previous Work:**
    - **Discriminator architecture**  
        [Elazar and Goldberg (2018)](https://arxiv.org/pdf/1808.06640.pdf) explore different capacities of adversarial component by increasing the `adv_hidden_size`, and show that there is no significant difference. Moreover, [Stacey et al. (2020)](https://aclanthology.org/2020.emnlp-main.665.pdf) investigate the influence of varying `adv_n_hidden` and `adv_activation_function`, and show that even a linear adversary (single layer MLP with a linear activation function) leads to similar results of using a more complex adversary. 
    - **Adv level**  
        The most common choice of `adv_level` is the last hidden layer, for example, a LSTM model followed by a output layer (`--last_hidden last_hidden`). We take the LSTM output as the input to adversary. [Wang et al. (2019)](https://arxiv.org/pdf/1811.08489.pdf) also consider adding adversaries at different levels, for example at the input level, which leads to a mask for the inputs (`--last_hidden input`), and at the intermediate level of the encoder. However, their experiments show that last hidden layer (final convolutional layer of ResNet-50) consistently outperform other methods. [Wadsworth et al. (2018)](https://arxiv.org/abs/1807.00199) also train a different variant which takes logits as input and even argue that inputs from hidden layers are not necessary. We implement this type as `--last_hidden output`.
    - **Update frequency**  
        There are mainly two types of adversarial training strategies: (1) GAN-style, which iteratively train the discriminator to identify protected attributes, and train the main model to make predictions while unlearn the discriminator (`--adv_update_frequency Epoch`); and (2) using Gradient Reversal Layer during backpropagation, which formulate the adversarial training as a multi-task learning that train the main model and adversarial component jointly (`--adv_update_frequency Batch`).
    - **Lambda**  
        There is no doubt that lambda is the most important hyperparameter of adversarial learning, denoting the strength of adversarial regularization. By setting lambda to 0, any adversarial training would degrade to a vanilla model. 
- **Tuned:**
  - Lambda (`adv_lambda`): log-uniformly grid search between 10^-3 ~ 10^3 with 60 trials.  
  - Update frequency (`adv_update_frequency`): try both Bach and Epoch update.

- **Not Tuned:**  
  - Adv level (`adv_level`): Most previous work has used `last_hidden` in their projects, and [Wang et al. (2019)](https://arxiv.org/pdf/1811.08489.pdf) have shown that `last_hidden` works the best in a complex NN, we follow the same setting.
  - Discriminator architecture: Previous work have demonstrated that different model architectures lead to almost identical results, so we didn't tune these models. 
  
  Another reason of not tuning adv level and discriminator architecture related hyperparameters is that, these hyperparameters need to be tuned jointly with lambda, which is too much expensive. 

- **Results**
    <p align="center">
        <img src="./../analysis/plots/Moji_Adv_Epoch-verse-Batch-update.png" width="800"/>
    </p>

### DAdv
- **Intro:** 
DAdv is a variant of Adv, which employs multiple adversaries and encourages each adversaries to learn a different aspect of protected 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**
### Augmented Adv & DAdv
- **Intro:** 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**

## INLP

- **Intro:** 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**


## FairBatch

- **Intro:** 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**


## Balanced Training

- **Intro:** 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**

## Group Difference

- **Intro:** 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**


## FairSCL

- **Intro:** 

- **Hyperparameters:**

- **Previous Work:**

- **Tuned:**

- **Not Tuned:**
