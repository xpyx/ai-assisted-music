### virtual machine environment setup

1. change rights
```
sudo chown -R $USER:$USER /opt/conda/pkgs/cache/
```

2. create a conda environment
```
conda create -n prism-samplernn anaconda
```

3. activate the created environment
```
conda activate prism-samplernn
```

4. install dependencies
```
pip install -r requirements.txt
```

5. install sndfile
```
sudo apt-get install libsndfile1
```

6. install sox
```
sudo apt install sox
```

7. clone the <a href="https://github.com/rncm-prism/prism-samplernn">PRiSM SampleRNN</a> repository
```
git clone https://github.com/rncm-prism/prism-samplernn.git
```

### training data preprocessing

7. cd to the directory where your samples are and change files to mono
```
for i in *wav; do sox $i ${i}_mono_merged.wav remix 1,2; done
```

8. (for the new files) resample to 16000hz
```
find . -maxdepth 1 -name '*mono_merged.wav' -type f -print0 | xargs -0 -t -I {} sox {} -r 16000 16000/{}
```

9. combine the files in the directory 16000/
```
cd 16000
sox -t wav *.wav combined.wav
```

10. move all combined wavs to one folder and combine them
```
sox -t wav *.wav combined_all.wav
```

11. move combined_all.wav to gce
```
gcloud compute scp combined_all.wav INSTANCE_NAME:~/prism-samplernn/input_audio
```

12. activate the created environment
```
conda activate prism-samplernn
```

13. activate the created environment
```
conda activate prism-samplernn
```
