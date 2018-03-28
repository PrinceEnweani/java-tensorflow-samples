# java-tensorflow-samples

Java sample codes on how to load tensorflow pretrained model file and predict based on these pretrained model files

# Usage

### Image Classification using Cifar10

Below show the [demo codes](image-classifier/src/main/java/com/github/chen0040/tensorflow/classifiers/images/Cifar10ImageClassifierDemo.java)
of the  Cifar10ImageClassifier which loads the [cnn_cifar10.pb](image-classifier/src/main/resources/tf_models/cnn_cifar10.pb)
tensorflow model file, and uses it to do image classification:

```java
package com.github.chen0040.tensorflow.classifiers.demo;

import com.github.chen0040.tensorflow.classifiers.utils.ResourceUtils;
import com.github.chen0040.tensorflow.classifiers.cifar10.Cifar10ImageClassifier;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.awt.image.BufferedImage;
import java.io.IOException;
import java.io.InputStream;

public class Cifar10ImageClassifierDemo {

    private static final Logger logger = LoggerFactory.getLogger(Cifar10ImageClassifierDemo.class);

    public static void main(String[] args) throws IOException {


        InputStream inputStream = ResourceUtils.getInputStream("tf_models/cnn_cifar10.pb");
        Cifar10ImageClassifier classifier = new Cifar10ImageClassifier();
        classifier.load_model(inputStream);

        String[] image_names = new String[] {
                "airplane1",
                "airplane2",
                "airplane3",
                "automobile1",
                "automobile2",
                "automobile3",
                "bird1",
                "bird2",
                "bird3",
                "cat1",
                "cat2",
                "cat3"
        };

        for(String image_name :image_names) {
            String image_path = "images/cifar10/" + image_name + ".png";
            BufferedImage img = ResourceUtils.getImage(image_path);
            String predicted_label = classifier.predict_image(img);
            logger.info("predicted class for {}: {}", image_name, predicted_label);
        }
    }
}
```

### Image Classification using Inception 

Below show the [demo codes](image-classifier/src/main/java/com/github/chen0040/tensorflow/classifiers/images/InceptionImageClassifierDemo.java)
of the  InceptionImageClassifier which loads the [tensorflow_inception_graph.pb](image-classifier/src/main/resources/tf_models/tensorflow_inception_graph.pb)
tensorflow model file, and uses it to do image classification:

```java
package com.github.chen0040.tensorflow.classifiers.demo;

import com.github.chen0040.tensorflow.classifiers.inception.InceptionImageClassifier;
import com.github.chen0040.tensorflow.classifiers.utils.ResourceUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.awt.image.BufferedImage;
import java.io.IOException;

public class InceptionImageClassifierDemo {

    private static final Logger logger = LoggerFactory.getLogger(InceptionImageClassifierDemo.class);

    public static void main(String[] args) throws IOException {


        InceptionImageClassifier classifier = new InceptionImageClassifier();
        classifier.load_model(ResourceUtils.getInputStream("tf_models/tensorflow_inception_graph.pb"));
        classifier.load_labels(ResourceUtils.getInputStream("tf_models/imagenet_comp_graph_label_strings.txt"));

        String[] image_names = new String[] {
                "tiger",
                "lion"
        };

        for(String image_name :image_names) {
            String image_path = "images/inception/" + image_name + ".jpg";
            BufferedImage img = ResourceUtils.getImage(image_path);
            String predicted_label = classifier.predict_image(img);
            logger.info("predicted class for {}: {}", image_name, predicted_label);
        }
    }
}
```

### Sentiment Analysis using 1D CNN

Below show the [demo codes](sentiment-analysis/src/main/java/com/github/chen0040/tensorflow/classifiers/sentiment/CnnSentimentClassifierDemo.scala)
of the  CnnSentimentClassifier which loads the [wordvec_cnn.pb](sentiment-analysis/src/main/resources/tf_models/wordvec_cnn.pb)
tensorflow model file, and uses it to do sentiment analysis:

```java
import com.github.chen0040.tensorflow.classifiers.sentiment.models.CnnSentimentClassifier;
import com.github.chen0040.tensorflow.classifiers.sentiment.utils.ResourceUtils;

import java.io.IOException;
import java.util.List;

public class CnnSentimentClassifierDemo {
    public static void main(String[] args) throws IOException {
        CnnSentimentClassifier classifier = new CnnSentimentClassifier();
        classifier.load_model(ResourceUtils.getInputStream("tf_models/wordvec_cnn.pb"));
        classifier.load_vocab(ResourceUtils.getInputStream("tf_models/wordvec_cnn.csv"));

        List<String> lines = ResourceUtils.getLines("data/umich-sentiment-train.txt");
        for(String line : lines){
            String label = line.split("\t")[0];
            String text = line.split("\t")[1];
            float[] predicted = classifier.predict(text);
            String predicted_label = classifier.predict_label(text);
            System.out.println(text);
            System.out.println("Outcome: " + predicted[0] + ", " + predicted[1]);
            System.out.println("Predicted: " + predicted_label + " Actual: " + label);
        }
    }
}


```

### Sentiment Analysis using Bi-directional LSTM

Below show the [demo codes](sentiment-analysis/src/main/scala/com/github/chen0040/tensorflow/classifiers/sentiment/BidirectionalLstmSentimentClassifierDemo.scala)
of the  BidirectionalLstmSentimentClassifier which loads the [wordvec_bidirectional_lstm.pb](sentiment-analysis/src/main/resources/tf_models/wordvec_bidirectional_lstm.pb)
tensorflow model file, and uses it to do sentiment analysis:

```java
import com.github.chen0040.tensorflow.classifiers.sentiment.models.BidirectionalLstmSentimentClassifier;
import com.github.chen0040.tensorflow.classifiers.sentiment.utils.ResourceUtils;

import java.io.IOException;
import java.util.List;

public class BidirectionalLstmSentimentClassifierDemo {
    public static void main(String[] args) throws IOException {
        BidirectionalLstmSentimentClassifier classifier = new BidirectionalLstmSentimentClassifier();
        classifier.load_model(ResourceUtils.getInputStream("tf_models/bidirectional_lstm_softmax.pb"));
        classifier.load_vocab(ResourceUtils.getInputStream("tf_models/bidirectional_lstm_softmax.csv"));

        List<String> lines = ResourceUtils.getLines("data/umich-sentiment-train.txt");

        for(String line : lines){
            String label = line.split("\t")[0];
            String text = line.split("\t")[1];
            float[] predicted = classifier.predict(text);
            String predicted_label = classifier.predict_label(text);
            System.out.println(text);
            System.out.println("Outcome: " + predicted[0] + ", " + predicted[1]);
            System.out.println("Predicted: " + predicted_label + " Actual: " + label);
        }
    }
}


```


