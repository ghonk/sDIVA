```

% % % 
% this lovely intro applies to NCONAWAY's DIVA, there are *in-progress* adaptations in this repo
% % % % % % % % % % % % % % % % % % % % % % % % % % % % %   ^^^ ¯\_(ツ)_/¯

% % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % %
% %         _     _      _     _      _     _      _     _          % %
% %        (c).-.(c)    (c).-.(c)    (c).-.(c)    (c).-.(c)         % %
% %         / ._. \      / ._. \      / ._. \      / ._. \          % %
% %       __\( Y )/__  __\( Y )/__  __\( Y )/__  __\( Y )/__        % %
% %      (_.-/'-'\-._)(_.-/'-'\-._)(_.-/'-'\-._)(_.-/'-'\-._)       % %
% %         || D ||      || I ||      || V ||      || A ||          % %
% %       _.' `-' '._  _.' `-' '._  _.' `-' '._  _.' `-' '._        % %
% %      (.-./`-'\.-.)(.-./`-'\.-.)(.-./`-'\.-.)(.-./`-'\.-.)       % %
% %       `-'     `-'  `-'     `-'  `-'     `-'  `-'     `-'        % %
% %                                                                 % %
% %       Written by Nolan Conaway (nconawa1@binghamton.edu).       % %
% % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % % %
```

This set of scripts runs a minimal version of the DIVA model of category learning (Kurtz, 2007). It is written in MATLAB, and is currently set up to simulate the six-types problem of Shepard, Hovland, & Jenkins (1961)--though it should generalize to any dataset. There are a variety of utility scripts, and a few important ones:

- **START.m** can be used to test DIVA using particular parameter sets.
- **DIVA.m** uses a provided architecture to train a network on a set of inputs and category assignments.
- **FORWARDPASS.m** and **BACKPROP.m** are used to propagate activations forward through the model, and error backward through the model, respectively. BACKPROP.m additionally completes a weight update.
- **responserule.m** implements DIVA's response rule, including its new focusing component.

Simulations are run by executing the *START.m* script. All simulations begin by passing a model struct to the *DIVA.m* function. At a minimum, 'model' needs to include:

```
model.betavalue __________ beta parameter for focusing
model.input ______________ an [eg,dimension] matrix of training exemplars
model.labels _____________ a column vector of class labels for each input
model.learningrate _______ learning rate for gradient descent
model.numblocks __________ number of passes through the training set
model.numhiddenunits _____ option for number of hidden units
model.numinitials ________ number of randomized divas
model.outputrule _________ output activation rule option.
model.weightrange ________ range of initial weight values
```

For almost all situations, inputs should be scaled to [-1 +1]. However, the target activations should be scaled to [0 1], in order to permit logistic output units. By default, the program automatically computes targets as scaled versions of the inputs. This is done in *DIVA.m*

By default, DIVA uses linear output units for continuous datasets, and logistic outputs for binary datasets. This option is set using model.outputrule: 'sigmoid' for logistic units, other values will result in linear. When linear outputs are used, SSE will be the error measure. Cross-entropy error is used for logistic outputs. Hidden units are always logistic.

DIVA.m will train the network and return a result struct. As-is, 'result' contains only training accuracy for each initialization at each training block. Additional measures, such as test phase classification, can be added. You will need to write custom code to compare DIVA's performance to a set of behavioral data.

*Nolan Conaway (nconawa1@binghamton.edu)* 

*November 10, 2014*
