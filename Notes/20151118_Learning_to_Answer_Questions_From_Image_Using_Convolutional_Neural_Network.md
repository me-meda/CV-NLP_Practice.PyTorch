## [Learning to Answer Questions From Image Using Convolutional Neural Network](http://arxiv.org/abs/1511.05756)

In this paper, we propose to employ the convolutional neural network (CNN) for the image question answering (QA). Our proposed CNN provides an end-to-end framework with convolutional architectures for learning not only the image and question representations, but also their inter-modal interactions to produce the answer. More specifically, our model consists of three CNNs: one image CNN to encode the image content, one sentence CNN to compose the words of the question, and one multimodal convolution layer to learn their joint representation for the classification in the space of candidate answer words. We demonstrate the efficacy of our proposed model on the DAQUAR and COCO-QA datasets, which are two benchmark datasets for the image QA, with the performances significantly outperforming the state-of-the-art.

# Introduction

Recently, the multimodal learning between image and language  has become an increasingly popular research area of artificial intelligence (AI). In particular, there have been rapid progresses on the tasks of bidirectional image and sentence retrieval , and automatic image captioning. In order to further advance the multimodal learning and push the boundary of AI research, a new "AI-complete" task, namely the visual question answering (VQA)  or image question answering  ,  is recently proposed. Generally, it takes an image and a free-form, natural-language like question about the image as the input and produces an answer to the image and question.


Image QA differs with the other multimodal learning tasks between image and sentence, such as the automatic image captioning. The answer produced by the image QA needs to be conditioned on both the image and question. As such, the image QA involves more interactions between image and language.  the image contents are complicated, containing multiple different objects. The questions about the images are very specific, which requires a detailed understanding of the image content. For the question "$\text{small what is the largest blue object in this picture?}$", we need not only identify the blue objects in the image but also compare their sizes to generate the correct answer. For the question "$\text{small how many pieces does the curtain have?}$", we need to identify the object  "$\text{small curtain}$" in the non-salient region of the image and figure out its quantity.

A successful image QA model needs to be built upon good representations of the image and question. Recently, deep neural networks have been used to learn image and sentence representations. In particular, convolutional neural networks (CNNs) are extensively used to learn the image representation for image recognition . CNNs  and recurrent neural networks (RNNs). also demonstrate their powerful abilities on the sentence representation for paraphrase, sentiment analysis, and so on. Moreover, deep neural networks  are used to capture the relations between image and sentence for image captioning and retrieval. However, for the image QA task, the ability of CNN has not been studied.




In this paper, we employ CNN to address the image QA problem. Our proposed CNN model, trained on a set of triplets consisting of (image, question, answer),  can answer free-form, natural-language like questions about the image. Our main contributions are:

1. We propose an end-to-end CNN model for learning to answer questions about the image. Experimental results on public image QA datasets show that our proposed CNN model surpasses the state-of-the-art.
2. We employ convolutional architectures to encode the image content, represent the question, and learn the interactions between the image and question representations, which are jointly learned to produce the answer conditioning on the image and question.


# Related Work

## Visual Turing Test

Recently, the visual Turing test, an open domain task of question answering based on real-world images, has been proposed to resemble the famous Turing test. In (["Are you talking to a machine? dataset and methods for multilingual image question answering"]()) a human judge will be presented with an image, a question, and the answer to the question by the computational models or human annotators. Based on the answer, the human judge needs to determine whether the answer is given by a human (i.e. pass the test) or a machine (i.e. fail the test). Geman et al. (["Visual turing test for computer vision systems"]()) proposed to produce a stochastic sequence of binary questions from a given test image, where the answer to the question is limited to yes/no. Malinowski et al. (["Towards a visual turing challenge"]())(["Hard to cheat: {A} turing test based on answering questions about images"]()) further discussed the associated challenges and issues with regard to visual Turing test, such as the vision and language representations, the common sense knowledge, as well as the evaluation.


## Image Question Answering

The image QA task, resembling the visual Turing test, is then proposed. Malinowski et al. (["A multi-world approach to question answering about real-world scenes based on uncertain input"]()) proposed a multi-world approach that conducts the semantic parsing of question and segmentation of image to produce the answer. Deep neural networks are also employed for the image QA task, which is more related to our research work.

The work  by (["Hard to cheat: {A} turing test based on answering questions about images"](),["Are you talking to a machine? dataset and methods for multilingual image question answering"]()) formulates the image QA task as a generation problem.

Malinowski et al.'s model (["Malinowski:2015b}, namely the Neural-Image-QA, feeds the image representation from CNN and the question into the long-short term memory (LSTM) to produce the answer. This model ignores the different characteristics of questions and answers. Compared with the questions, the answers tend to be short, such as one single word denoting the object category, color, number, and so on.

The deep neural network in (["Are you talking to a machine? dataset and methods for multilingual  image question answering"]()), inspired by the multimodal recurrent neural networks model (["Explain images with multimodal recurrent neural networks"](),["Ask your neurons: {A} neural-based approach to answering questions about images"]()), used two LSTMs for the representations of question and answer, respectively.

In (["Exploring models and data for image question answering"]()), the image QA task is formulated as a classification problem, and the so-called visual semantic embedding (VSE) model is proposed. LSTM is employed to jointly model the image and question by treating the image as an independent word, and appending it to the question at the beginning or ending position. As such, the joint representation of image and question is learned, which is further used for classification. However, simply treating the image as an individual word cannot help effectively exploit the complicated relations between the image and question. Thus, the accuracy of the answer prediction may not be ensured.

In order to cope with these drawbacks, we proposed to employ an end-to-end convolutional architectures for the image QA to capture the complicated inter-modal relationships as well as the representations of image and question. Experimental results demonstrate that the convolutional architectures can  can achieve better performance for the image QA task.




# Proposed CNN for Image QA


The proposed CNN model for image QA

![](http://conviqa.noahlab.com.hk/ConvIQA.png)


For image QA, the problem is to predict the answer $a$ given the question $q$ and the related image $$I$$:
$$
  a = arg \max_{a\in \Omega}~p(a|q,I;{\theta}),
$$

where $\Omega$ is the set containing all the answers. ${\theta}$ denotes all the parameters for performing image QA. In order to make a reliable prediction of the answer, the question $q$ and image $I$  need to be adequately represented.  Based on their representations, the relations between the two multimodal inputs are further learned to produce the answer.  In this paper, the ability of CNN is exploited for not only modeling image and sentence individually, but also capturing the relations and interactions between them.



As illustrated in Figure above, our proposed CNN framwork for image QA consists of three individual CNNs: one image CNN encoding the image content, one sentence CNN generating the question representation, one multimodal convolution layer fusing the image and question representations together and generate the joint representation. Finally, the joint representation is fed into a softmax layer to produce the answer. The three CNNs and softmax layer are fully coupled for our proposed end-to-end image QA framework, with all the parameters (three CNNs and softmax) jointly learned in an end-to-end fashion.


## Image CNN}

There are many research papers employing CNNs to generate image representations, which achieve the state-of-the-art performances on image recognition  (["Simonyan:14,Szegedy:2014}. In this paper, we employ the work (["Very deep convolutional networks for large-scale image recognition"]()) to encode the image content for our image QA model:
$$
  \nu_{im} = \sigma (\mathbf{w}_{im}(CNN_{im}(I))+b_{im}),
$$
where $\sigma$ is a nonlinear activation function, such as Sigmoid and ReLU (["Dahl:2013}. $CNN_{im}$ takes the image as the input and outputs a fixed length vector as the image representation. In this paper, by chopping out the top softmax layer and the last ReLU layer of the CNN (["Very deep convolutional networks for large-scale image recognition"]()), the output of the last fully-connected layer is deemed as the image representation, which is a fixed length vector with dimension as 4096. Note that $\mathbf{w}_{im}$ is a mapping matrix of the dimension $d\times4096$, with $d$ much smaller than $4096$. On one hand, the dimension of the image representation is reduced from 4096 to $d$. As such, the total number of parameters for further fusing image and question, specifically the multimodal convolution process, is significantly reduced.

Consequently, fewer samples are needed for adequately training our CNN model. On the other hand, the image representation is projected to a new space, with the nonlinear activation function $\sigma$ increasing the nonlinear modeling property of the image CNN. Thus its capability for learning complicated representations is enhanced. As a result, the multimodal convolution layer (introduced in the following section) can better fuse the question and image representations together and further exploit their complicated relations and interactions to produce the answer.



## Sentence CNN

In this paper, CNN is employed to model the question for image QA. As most convolution models, we consider the convolution unit with a local "receptive field" and shared weights to capture the rich structures and composition properties between consecutive words.  For a given question with each word represented as the word embedding (["Mikolov:13}, the sentence CNN with several layers of convolution and max-pooling is performed to generate the question representation $\nu_{qt}$.


$\textbf{Convolution}$: For a sequential input $\nu$, the convolution unit for feature map of type-$f$  on the $\ell^{th}$ layer is
$$
\nu_{(\ell, f)}^{i} \overset{\text{def}}{=}
\sigma(\mathbf{w}_{(\ell,f)} \vec{\nu}_{(\ell-1)}^{i} + b_{(\ell,f)}),
$$
where $\mathbf{w}_{(\ell, f)}$ are the parameters for the $f$ feature map on the $\ell^{th}$ layer, $\sigma$ is the nonlinear activation function, and $\vec{\nu}_{(\ell-1)}^{i}$ denotes the segment of $(\ell\hspace{-1pt}-\hspace{-1pt}1)^{th}$ layer for the convolution at location $i$ , which is defined as follows.
$$
\vec{\nu}_{(\ell-1)}^{i}  \overset{\text{def}}{=}  \nu_{(\ell-1)}^{i} \parallel \nu_{(\ell-1)}^{i+1} \parallel \cdots \parallel\nu_{(\ell-1)}^{i+s_{rp}-1},
$$
where $s_{rp}$ defines the size of local "receptive field" for convolution. "$\parallel$" concatenates the  $s_{rp}$  vectors into a long vector. In this paper, $s_{rp}$ is chosen as 3 for the convolution process. The parameters within the convolution unit are shared for the whole question with a window covering 3 semantic components sliding from the beginning to the end. The input of  the first convolution layer for the sentence CNN is the word embeddings of the question:
$$
\vec{\nu}_{(0)}^{i}  \overset{\text{def}}{=}  \nu_{wd}^{i} \parallel \nu_{wd}^{i+1} \parallel  \cdots \parallel\nu_{wd}^{i+s_{rp}-1},
$$
where $\nu_{wd}^{i}$ is the word embedding of the $i^{th}$ word in the question.


$\textbf{Max-pooling}$: With the convolution process, the sequential $s_{rp}$ semantic components are composed to a higher semantic representation. However, these compositions may not be the meaningful representations, such as "$\text{small is on the}$" of the question in Figure. The max-pooling process following each convolution process is performed:
$$
\begin{cases}
\nu^i_{(\ell+1,f)} = \max(\nu^{2i}_{(\ell,f)},\nu^{2i+1}_{(\ell,f)}).
\end{cases}
$$
Firstly, together with the stride as two, the max-pooling process shrinks half of the representation, which can quickly make the sentence representation. Most importantly, the max-pooling process can select the meaningful compositions while filter out the unreliable ones. As such, the meaningful composition "$\text{small of the chair}$" is more likely to be pooled out, compared with the composition "$\text{small front of the}$".

The convolution and max-pooling processes exploit and summarize the local relation signals between consecutive words. More layers of convolution and max-pooling can help to summarize the local interactions between words at larger scales and finally reach the whole representation of the question. In this paper, we employ three layers of convolution and max-pooling to generate the question representation $\nu_{qt}$.


## Multimodal Convolution Layer


The image representation $\nu_{im}$ and question representation $\nu_{qt}$ are obtained by the image and sentence CNNs, respectively. We design a new multimodal convolution layer on top of them, as shown in Figure \ref{fig:multimodal_CNN}, which fuses the multimodal inputs together to generate their joint representation for further answer prediction. The image representation is treated as an individual semantic component. Based on the image representation and the two consecutive semantic components from the question side, the mulitmodal convolution is performed, which is expected to capture the interactions and relations between the two multimodal inputs.
$$
\vec{\nu}_{mm}^{in}  \overset{\text{def}}{=}  \nu_{qt}^{i} \parallel \nu_{im} \parallel \nu_{qt}^{i+1},
$$
$$
\nu_{(mm, f)}^{i} \overset{\text{def}}{=}
\sigma(\mathbf{w}_{(mm,f)} \vec{\nu}_{mm}^{in} + b_{(mm,f)}),
$$
where $\vec{\nu}_{mm}^{in}$ is the input of the multimodal convolution unit. $\nu_{qt}^{i}$ is the segment of the question representation at location $i$. $\mathbf{w}_{(mm,f)}$ and $b_{(mm,f)}$ are the parameters for the type-$f$ feature map of the multimodal convolution layer.

Similarly, (["Show and tell: {A} neural image caption generator"]()) employed LSTM for the image caption generation by treating the image as the first word.

The bidirectional LSTM (["Exploring models and data for image question answering"]()) is performed  by appending the image in the beginning or end of the question, which make the image interact more closely with the words of the question. However, the relations between image and question are complicated. The image content may interact with high semantic representations composed from words. LSTM cannot effectively capture such interactions. As the image representation is treated as an individual word,  the effect of image will vanish with each time step of LSTM. As such, the relation between the image and high semantic representations composed from words cannot be well exploited. For our CNN model, the sentence CNN firstly composes the question to higher semantic components. With the end-to-end learning strategy, the multimodal convolution process can thus fuse the  semantic representations of image and question together  to adequately exploit their interactions, the ability of which will be demonstrated in the experiment section.

Alternatively, LSTM could be used to fuse the image and question representations, as in (["Ask your neurons: {A} neural-based approach to answering questions about images"]()),["Exploring models and data for image question answering"]()).  For example, in the latter work, a bidirectional LSTM (["Exploring models and data for image question answering"]()) is employed by appending the image representation to the beginning or ending position of the question. We argue that it is better to employ CNN than LSTM for the image QA task, due to the following reason, which has also been verified in the following experiment section. The relations between image and question are complicated. The image may interact with the high-level semantic representations composed from of a number of words. However, LSTM cannot effectively capture such interactions. Treating the image representation as an individual word, the effect of image will vanish at each time step of LSTM in (["Ren:2015}. As a result, the relations between the image and the high-level semantic representations of words may not be well exploited. In contrast, our CNN model can effectively deal with the problem. The sentence CNN first compose the question into a high-level semantic representations. The multimodal convolution process further fuse the semantic representations of image and question together and adequately exploit their interactions.


After the mutlimodal convolution layer, the multimodal representation $\nu_{mm}$ jointly modeling the image and question is obtained. $\nu_{mm}$ is then fed into a softmax layer as shown in Figure \ref{fig:imageqa}, which produces the answer to the given image and question pair.



# Experiments

In this section, we firstly introduce the configurations of our CNN model for image QA and how we train the proposed CNN model. Afterwards, the public image QA datasets and evaluation measurements are introduced. Finally, the experimental results are presented and analyzed.

## Configurations and Training

Three layers of convolution and max-pooling are employed for the sentence CNN. The numbers of the  feature maps for the three convolution layers are 300, 400, and 400, respectively. The sentence CNN is designed on a fixed architecture, which needs to be set to accommodate the maximum length of the questions. In this paper, the maximum length of the question is chosen as 38. The word embeddings are obtained by the skip-gram model (["Efficient estimation of word representations in vector space"]()) with the dimension as 50. We use the VGG (["Very deep convolutional networks for large-scale image recognition"]()) network as the image CNN. %By chopping out the top softmax layer and the last ReLU layer, the obtained image representation are nonlinearly mapped to a new representation.

The dimension of $\nu_{im}$ is set as 400. The multimodal CNN takes the image and sentence representations as the input and generate the joint representation with the number of feature maps as 400.

The proposed CNN model is trained with stochastic gradient descent with mini batches of 100 for optimization, where the negative log likelihood is chosen as the loss. During the training process, all the parameters are tuned, including the parameters of nonlinear image mapping, image CNN, sentence CNN, multimodal convolution layer, and  softmax layer. Moreover, the word embeddings are also fine-tuned. In order to prevent overfitting, dropout (with probability 0.1) is used.



## Image QA Datasets

We test and compare our proposed CNN model on the public image QA databases, specifically the DAQUAR (["A multi-world approach to question answering about real-world scenes  based on uncertain input"]() and COCO-QA (["Exploring models and data for image question answering"]() datasets.

$\textbf{DAQUAR-All}$ (["A multi-world approach to question answering about real-world scenes   based on uncertain input"]())  This dataset consists of 6,795 training and 5,673 testing samples, which are generated from 795 and 654 images, respectively. The images are from all the 894 object categories. There are mainly three types of questions in this dataset, specifically the object type, object color, and number of objects. The answer may be a single word or multiple words.

$\textbf{DAQUAR-Reduced}$ (["A multi-world approach to question answering about real-world scenes   based on uncertain input"]() This dataset is a reduced version of DAQUAR-All, comprising 3,876 training and 297 testing samples. The images are constrained to 37 object categories. Only 25 images are used for the testing sample generation. Same as the DAQUAR-All dataset, the answer may be a single word or multiple words.

$\textbf{COCO-QA}$ (["Exploring models and data for image question answering"]() This dataset consists of 79,100 training and 39,171 testing samples, which are generated from about 8,000 and 4,000 images, respectively. There are four types of questions, specifically the object, number, color, and location. The answers are all single-word.

## Evaluation Measurements}

One straightforward way for evaluating image QA is to utilize accuracy, which measures the proportion of the correctly answered testing questions to the total testing questions. Besides accuracy, Wu-Palmer similarity (WUPS) (["Verb semantics and lexical selection"]()["A multi-world approach to question answering about real-world scenes based on uncertain input"]()) is also used to measure the performances of different models on the image QA task. WUPS calculates the similarity between two words based on their common subsequence in a taxonomy tree. A threshold parameter is required for the calculation of WUPS. Same as the previous work (["Ren:2015,Malinowski:2014a,Malinowski:2015b}, the threshold parameters 0.0 and 0.9 are used for the measurements WUPS@0.0 and WUPS@0.9, respectively.


## Experimental Results and Analysis

### Competitor Models

We compare our models with recently developed models for the image QA task, specifically the multi-world approach (["A multi-world approach to question answering about real-world scenes based on uncertain input"]()), the VSE model (["Exploring models and data for image question answering"]()), and the Neural-Image-QA approach (["Ask your neurons: {A} neural-based approach to answering questions about images"]()).

For the DAQUAR-All dataset, we evaluate the performances of different image QA models on the full set ("multiple words"). The answer to the image and question pair may be a single word or multiple words.  Same as the work ([""Ask your neurons: {A} neural-based approach to answering questions   about images"]()), a subset containing the samples with only a single word as the answer is created and employed for comparison ("single word"). Our proposed CNN model significantly outperforms the multi-world approach and Neural-Image-QA in terms of accuracy, WUPS@0.0, and WUPS@0.9. Specifically, our proposed CNN model achieves over $20\%$ improvement compared to Neural-Image-QA in terms of accuracy on both "multiple words" and "single word".

Compared with the neural-based approach employing LSTM to model the image and question, our proposed CNN model uses the convolutional architectures to exploit the image representation, question representation, and their relations and interactions.

The results demonstrate that our CNN model can more accurately model the image and question as well as their interactions, thus yields better performances for the image QA task.

Moreover, we treat the answer comprising multiple words as an independent class. And the performance on "multiple words" is inferior to that on "single word".
Moreover, the language approach ([""Ask your neurons: {A} neural-based approach to answering questions   about images"]()), which only resorts to the question performs inferiorly to the approaches that jointly model the image and question. The image component is thus of great help to the image QA task. One can also see that the performances on "multiple words" are generally inferior to those on "single word".


For the DAQUAR-All dataset, we evaluate the performances of different image QA models on the full set ("multiple words"). In addition, we evaluate the performance of our CNN model on the samples which only contain a single word as answer ("single word"). Our proposed CNN model significantly outperforms the multi-world approach and the LSTM approach in terms of accuracy, WUPS@0.0, and WUPS@0.9. Specifically, our CNN model achieves over $20\%$ improvement compared to the LSTM approach in terms of accuracy on both "multiple words" and "single word".

The results demonstrate that our CNN model can more accurately model the image and question as well as their interactions, and thus yield better performances.Furthermore, the language approach  (["Malinowski:2015b} which only resorts to the question is inferior to the approaches that jointly model the image and question. The use of image is thus helpful for the image QA task. One can also see that the performances on "multiple words" are generally inferior to those on "single word".

For the DAQUAR-Reduced dataset, besides the neural-based approach ([""Ask your neurons: {A} neural-based approach to answering questions   about images"]()), the VSE model (["Exploring models and data for image question answering"]()) is also compared on "single word". Moreover, some of the methods introduced in (["Exploring models and data for image question answering"]()) are also reported here. GUESS is the method of randomly outputting an answer according to the question type. BOW treats each word of the question equally and sums all the word vectors to predict the answer by logistic regression. LSTM is performed only on the question without considering the image, which is similar to the language approach ([""Ask your neurons: {A} neural-based approach to answering questions   about images"]()). IMG+BOW performs multinomial logistic regression based on the image features and a BOW vector obtained by summing all the word vectors of the question. VIS+LSTM and 2-VIS+BLSTM  are two versions of the VSE model. VIS+LSTM has only a single LSTM to encode the image and question in one direction, while 2-VIS+BLSTM uses a bidirectional LSTM to encode the image and question along both directions. Our proposed CNN model can significantly outperform the competitor models. More specifically, for the case of "single word", our proposed CNN achieves nearly $20\%$ improvement in terms of accuracy over the best competitor model of 2-VIS+BLSTM.


## Influence of Multimodal Convolution Layer

The image and question needs to be considered together for the image QA. The multimodal convolution layer in our proposed CNN model not only fuses the image and question representations together but also learns the interactions and relations between the two multimodal inputs for further question prediction. The effect of the multimodal convolution layer is examined as follows. The image and question representations are simply concatenated together as the input of the softmax layer for the answer prediction. We train the network in the same manner as the proposed CNN model. Firstly, it can be observed that without the multimodal convolution layer, the performance on the image QA has dropped. Comparing to the simple concatenation process fusing the image and question representations, our proposed multimodal convolution layer can well exploit the complicated relationships between image and question representations. Thus a better performance for the answer prediction is achieved. Secondly, the approach without multimodal convolution layer outperforms the IMG+BOW, VIS+LSTM and 2-VIS+BLSTM, in terms of accuracy. The better performance is mainly attributed to the composition ability of the sentence CNN. Even with the simple concatenation process, the image representation and composed question representation can be fuse together for a better image QA model.

## Influence of Image CNN and Effectiveness of Sentence CNN

As can be seen from Table \ref{table:DAQUAR_all}, without the use of image content, the accuracy of human answering drops from $50\%$ to $12\%$. Therefore, the image content is critical to the image QA task. Similarly to the work in (["Malinowski:2015b,Ren:2015}, we only use the question representation generated from the Sentence CNN to predict the answer. The results are listed in Table \ref{table:COCO_QA}. Firstly, without the use of image representation, the performance of our proposed CNN model is significantly reduced, which again demonstrates the importance of using image component. Secondly, the model only consisting of the Sentence CNN performs better than LSTM and BOW. It indicates that the Sentence CNN is more effective for generating the question representation than LSTM and BOW. Recall that the concatenation approach outperforms IMG+BOW, VIS+LSTM, and 2-VIS+BLSTM, as explained above. It also demonstrates the effectiveness of the Sentence CNN.


Moreover, we examine the language modeling ability of the sentence CNN as follows. The words of the test questions are randomly reshuffled. Then the reformulated questions are sent to the sentence CNN to check whether the sentence CNN can still generate reliable question representations and make accurate answer predictions. For randomly reshuffled questions, the results on COCO-QA dataset are 40.74, 53.06, and 80.41 for the accuracy, WUPS@0.9, and WUPS@0.0, respectively, which are significantly inferior to that of natural-language like questions. The result indicates that the sentence CNN possesses the ability of modeling natural questions. The sentence CNN uses the convolution process to compose and summarize the neighboring words. And the reliable ones with higher semantic meanings will be pooled and composed further to reach the final sentence representation. As such, the sentence CNN can compose the natural-language like questions to reliable high semantic representations.


Next, we examine the language modeling ability of the Sentence CNN. The words of the test questions are randomly reshuffled and then the reformulated questions are sent to the Sentence CNN to check whether the model can still handle the questions and make reliable answer predictions. The results in terms of accuracy, WUPS@0.9, and WUPS@0.0 on the COCO-QA dataset are 40.74, 53.06, and 80.41, respectively, which are significantly inferior to those of the original natural questions. The result indicates that the Sentence CNN uses the convolution processes to compose and summarize the semantic components of consecutive words in the question and thus has the ability of modeling natural questions.


# Conclusion

In this paper, we proposed one CNN model to address the image QA problem. The proposed CNN model relies on convolutional architectures to generate the image representation, compose consecutive words to the question representation, and learn the interactions and relations between the image and question for the answer prediction. Experimental results on public image QA datasets demonstrate the superiority of our proposed model over the state-of-the-art methods.
