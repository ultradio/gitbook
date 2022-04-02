# Spark Development Environment

환경구축
```
## Scala  설치
sudo apt-get install scala

scala

export SCALA_HOME=/usr/bin/scala
export PATH=$SCALA_HOME/bin:$PATH

## Spark 설치
wget https://archive.apache.org/dist/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz

tar xvf spark-2.4.5-bin-hadoop2.7.tgz

sudo mv spark-2.4.5-bin-hadoop2.7/ /usr/spark

export SPARK_HOME=/usr/spark
export PATH=$SPARK_HOME/bin:$PATH

spark-shell

## Python 설치
sudo apt install python3
sudo apt install python3-pip

## Anaconda 설치
wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh
bash Anaconda3-2019.03-Linux-x86_64.sh
source ~/.bashrc

# conda create --name py3 python=3
conda create -y -n pyspark_conda_env -c conda-forge pyarrow pandas conda-pack python=3
conda activate pyspark_conda_env
conda pack -f -o pyspark_conda_env.tar.gz

conda install -n pyspark_conda_env ipykernel --update-deps --force-reinstall
conda activate py3

## Jupyter  설치
pip3 install jupyter
jupyter notebook --generate-config

# vim ~/.jupyter/jupyter_notebook_config.py
c.NotebookApp.token = ''
c.NotebookApp.password = u''
c.NotebookApp.open_browser = True
c.NotebookApp.ip = 'localhost'
c.NotebookApp.port = 8899

## 환경 설정
# vim ~/.bashrc
export SCALA_HOME=/usr/bin/scala
export PATH=$SCALA_HOME/bin:$PATH

export SPARK_HOME=/usr/spark
export PATH=$SPARK_HOME/bin:$PATH

alias python=python3

export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.7-src.zip:$PYTHONPATH
export PYSPARK_PYTHON=python3
```

참고자료

https://spark.apache.org/docs/latest/api/python/user_guide/sql/arrow_pandas.html#ensure-pyarrow-installed
