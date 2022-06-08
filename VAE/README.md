0. Record data
```
python record_data.py --max-steps 10000 -f  path-to-record/folder/ 
```

1. Train a VAE:
```
python -m vae.train --n-epochs 500 --verbose 0 --z-size 64 -f path-to-record/folder/
```

## Explore Latent Space

```
python -m vae.enjoy_latent -vae logs/level-0/vae-8.pkl
```

