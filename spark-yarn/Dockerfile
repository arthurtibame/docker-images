FROM cithub/hadoop
MAINTAINER Channel IT Services, LLC

ENV SPARK_FILE_VER=2.4.5-bin-hadoop2.7
ENV SPARK_VER=2.4.5
ENV SPARK_HOME=/home/hadoop/spark/spark
ENV SPARK_OPTS=-"--packages graphframes:graphframes:0.7.0-spark2.3-s_2.11,com.databricks:spark-csv_2.11:1.5.0"
ENV YARN_CONF_DIR=/home/hadoop/hadoop/etc/hadoop

VOLUME	/data

RUN\
	apt-get update &&\
	mkdir -p /home/hadoop/spark &&\
	wget https://www.apache.org/dyn/closer.lua/spark/spark-$SPARK_VER/spark-$SPARK_FILE_VER.tgz &&\
	tar xzvf spark-$SPARK_FILE_VER.tgz &&\
	mv spark-$SPARK_FILE_VER $SPARK_HOME &&\
	rm spark-$SPARK_FILE_VER.tgz
RUN\
	apt-get update &&\
	apt-get install -y python3-pip python3-numpy python3-scipy python3-matplotlib python3-pandas python3-sympy python3-nose &&\
	apt-get install -y python3-dev python3-pip g++ libopenblas-dev git &&\
	apt-get clean &&\
	pip3 install jupyter &&\
	ln -sf /usr/bin/python3 /usr/bin/python &&\
	pip3 install --upgrade pip &&\
	pip3 install spaCy &&\
	pip3 install pyLDAvis scipy gensim nltk bokeh py4j &&\
	pip3 install -U scikit-learn &&\
	pip3 install toree &&\
	pip3 install tensorflow &&\
	pip3 install keras &&\
	jupyter toree install --replace --interpreters=Scala,PySpark --spark_home=$SPARK_HOME --spark_opts=$SPARK_OPTS
RUN\
	wget -q https://raw.githubusercontent.com/channelit/docker-images/master/spark-yarn/firststart.sh -O /firststart.sh &&\
	chmod 755 firststart.sh

# Workaround to add graphframes to python jupyter
RUN\
	touch /home/hadoop/spark/spark/conf/spark-env.sh &&\
	echo "#!/bin/sh" >> /home/hadoop/spark/spark/conf/spark-env.sh &&\
	echo "export PYSPARK_PYTHON=python" >> /home/hadoop/spark/spark/conf/spark-env.sh &&\
	echo "export PYTHONPATH=$PYTHONPATH:/home/hadoop/spark/spark/graphframes.jar:." >> /home/hadoop/spark/spark/conf/spark-env.sh

EXPOSE 8888 4040 9000 8031 8032 8033 7077 8080 8081
