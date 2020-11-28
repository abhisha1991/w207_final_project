FROM tensorflow/tensorflow:2.2.0-gpu

RUN apt-get update
RUN apt-get install -y git
RUN pip3 install -U pip
RUN pip3 install pandas
RUN pip3 install numpy
RUN pip3 install -U scikit-learn
RUN pip3 install keras==2.4.2
RUN pip3 install jupyter
RUN pip3 install -U tf-hub-nightly
RUN pip3 install matplotlib

# RUN mkdir /project
WORKDIR /project
RUN git clone https://github.com/abhisha1991/w207_final_project.git

EXPOSE 8888
CMD jupyter notebook  --no-browser --ip=0.0.0.0 --allow-root