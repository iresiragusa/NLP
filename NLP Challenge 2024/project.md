# **Hints**

## Esame

Ai fini dell'esame dovrete mandare via e-mail i notebook (uno per task) usati per il pre-processing dei dati, l'addestramento dei modelli e la generazione delle predizioni e i punteggi ottenuti sulla leaderboard.

I notebook dovranno essere già eseguiti e commentati, illustrando i vostri ragionamenti e scelte architetturari. È necessario che il team leader invii la mail a nome del gruppo con oggetto _[NLP] consegna esame NOME_GRUPPO_ 

L’accesso alla piattaforma per la sottomissione dei risultati e la consegna del progetto ha una deadline fissata entro la fine di marzo, così da poter consegnare il progetto entro le tre sessioni invernale e la straordinaria di aprile.

### Submission leaderboard

Sulla piattaforma Palantir Foundry di OpenDataPlayground, i file devono essere sottomessi secondo questi formati:

  - irony -> csv file w/ *only* predictions only
  - emotion -> csv file w/ header (label) + predictions

Se avete dubbi, controllate i file Example_Prediction_Submission.csv di entrambi i task. 

## Binary classification vs Multi-class classification
  
  Richiedono considerazioni diverse in termini di activation function + loss function ([more info](https://medium.com/analytics-vidhya/activation-functions-and-loss-functions-for-neural-networks-how-to-pick-the-right-one-542e1dd523e0)):
  
  - Classificazione binaria -> Una unità di uscita con sigmoide e la loss può essere la cross-entropia binaria;
  
  - Classificazione multi-classe con N classi -> N unità di uscita con softmax (attenzione al parametro dim in torch), la classe vincitrice la calcoliamo tramite argmax e la loss è la cross-entropia multi-classe.

  Per ogni loss controlliamo se stiamo usando una loss che dentro ha la funzione di attivazione o meno:

  - Loss + activation -> non ho activation function nella rete, se voglio calcolare manualmente accuracy/precision/recall in addestramento, devo generare manualmente le logits, applicando la funzione di attivazione;
  
  - Loss -> inserisco all'interno della rete l'activation function, l'output della rete sono le logits.

  ***! Attenzione !***

  - Posso applicare una threshold alle logits ottenute dall'applicazione della sigmoide per determinare la classe predetta. La loss si applica alle logits, la threshold FUORI DALLA FASE DI ADDESTRAMENTO (non subisce backpropagation) e solo per generare TP e TN per il calcolo delle metriche di accuracy/precision/recall.

  - Nel calcolo di misure quali accuracy/precision/recall o loss per epoca, attenzione al denominatore: len(dataloader) restituisce il numero di batch mentre len(dataset) il numero dei sample.

## Quali modelli usare

  Su huggingface sono presenti diversi modelli di tipo encoder-only, encoder-decoder e decoder-only che potete usare. Modelli diversi hanno tokenizer e pesi diversi e sono stati addestrati su diversi dati e con diverse strategie. Assicuratevi che i modelli che utilizzate non siano stati addestrati per il task di rilevazione di sentiment/irony su tweet, guardando il paper verificate che non ci siano "contaminazioni" nei dataset di addestramento.

   ***! Attenzione !*** Alcuni modelli possono variare con versione cased/uncased, base/large, in particolare i modelli large non restituiscono un embedding di 768 elementi, e verificate anche la grandezza dell'input che tali modelli richiedono in ingresso.

## Emoji

  Su python è possibile interagire con le emoji attraverso alcune librerie come [emoji](https://github.com/kyokomi/emoji) e [emot](https://github.com/NeelShah18/emot). Inoltre alcuni tokenizer dei modelli BERT-like hanno un token associato alle diverse emoji o, quando questo non è presente, è possibile aggiungere nuovi token e permettere al modello di apprendere una nuova rappresentazione tramite fine-tuning del modello. Questo si fa tramite la funzione add_tokens del tokenizer ed è necessario il resize del modello [more info + esempio](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizer.add_tokens)


Potete contattarmi via mail irene.siragusa02@unipa.it
