# Google-AI4Code

Training and inference code for my solo silver medal in Google AI4Code at Kaggle. 

Model architecture
![image](https://user-images.githubusercontent.com/22745975/192646694-19ebbb63-a317-47ea-be02-572e4776b0d8.png)

The model is based on treating the problem as a regression task, where the known relative code cell positions are assigned a value between 0 and 1 and the predicted markdown cell positions can take a value between -0.2 and 1.2, this way I impose an inductive bias on the code cells and allow the markdown cells to be place anywhere before and after the code cells.

Firstly, the tokenized (CodeBERT tokenizer) code and markdown cells are both fed separately into CodeBERT. Only the first latent code and markdown output vectors from CodeBert are selected. Then, positional encodings are added to the latent code vector, this is the input to the encoder. The output from the encoder is then input (as context) alongside the latent markdown vector into the decoder which can attend to the interactions between all cells, code and markdown. The output of the decoder is finally fed into the regressor which outputs the markdown cell position predictions.

Model Training

The models were trained only with the dataset provided by the competition. The only notable difference in training with regards to other solutions is that instead of using MSE or MAE as a loss function I tried to approximate Kendall Tau using a loss function inspired by Spearman Rho which internally uses softrank, making it differentiable.

Closing thoughts

Having entered the competition only one month prior to the deadline I ran out of computational power and time, therefore my model ended up being undertrained.

I really enjoyed this competition, see you guys on the next one!




UPDATE (25/10/22): 

After doing some experiments with the Kendall Tau Loss approximation and the original Kendall Tau, I noticed a mistake in the training notebook that impairs the quality of the approximation across notebooks of different sizes. 

Original:

return 4 * (d2/ ((n * (n-1))))

Improved: 

return 6 * (d2/ ((n * (n**2 -1)))) 
