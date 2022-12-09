# ML_Cyber_Lab2_Report

## Github Repository

https://github.com/YingkaiHao/ML_Cyber_Lab2

## Result

### Performance of the original model on test data

| Clean Classification Accuracy | Attack Success Rate |
| ----------------------------- | ------------------- |
| 98.62%                        | 100%                |

### Performance of the repaired model on test data

| validation accuracy drop  X% | Clean Classification Accuracy | Attack Success Rate |
| ---------------------------- | ----------------------------- | ------------------- |
| x = 2                        | 95.90%                        | 100%                |
| x = 4                        | 92.29%                        | 99.98%              |
| x = 10                       | 84.54%                        | 77.21%              |

### Performance of the repaired_net (G) on test data

| validation accuracy drop  X% | Clean Classification Accuracy | Attack Success Rate |
| ---------------------------- | ----------------------------- | ------------------- |
| x = 2                        | 95.74%                        | 100%                |
| x = 4                        | 92.13%                        | 99.98%              |
| x = 10                       | 84.33%                        | 77.21%              |

## Conclusion

In this lab, we aim to design a backdoor detector for BadNets trained on the YouTube Face dataset using the pruning defense. My detector will take B (a backdoor neural network classifier with N classes) and D_valid (a validation dataset of clean, labelled images) as input. And the output G is a "repaired" BadNet. G has N + 1 classes, and given unseen test input, it must output the correct class if the test input is clean (the correct class will be in[1, N]) and class N + 1 if the input is backdoored.

As you can see above, I have concluded all performance result. Some interesting facts are found. 

First, the attack success rate is higher than clean classification accuracy, which means that this BadNet model works perfect on attack. 

Second, as long as I did the pruning defense, I found that the clean classification accuracy drop faster than attack success rate before the validation accuracy drops at least 4% below the original accuracy. This means that we are dealing with a **pruning aware attack**[1]. This type of attack operates in 4 steps. In step 1, the attacker trains the baseline model using a clean training dataset. In step 2, the attacker prunes the model by eliminating dormant neurons. In step 3, the attacker retraines the pruned model by using poisoned training dataset. This retrained model works perfect on both clean inputs and poisoned inputs. In step 4, the attacker reinstates all pruned neurons back into the network along with the associated weights and biases. Therefore, when we deal with a pruning aware attack, using only pruning defense is not enough. 

Third, when the validation accuracy drops to at least 10%, we find that the attack success rate is lower than clean classification accuracy now. Although the attack success rate is lower than clean classification accuracy, the clean classification accuracy is not high enough and the attack success rate is not low enough. Since that attacker used pruning aware attack and same neurons work for both clean data and posioned data, this result can be predicted. 

Finally, we can conclude that using pruning defense is not too effective in this lab. In order to maintain high clean classification accuracy and low attack success rate, maybe a **fine-pruning defense**[1] is needed in this case. 

## Reference

[1] Liu, K., Dolan-Gavitt, B., Garg, S. (2018). Fine-Pruning: Defending Against Backdooring Attacks on Deep Neural Networks. International Symposium on Recent Advances in Intrusion Detection.