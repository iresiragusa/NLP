# **Hints**

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

  Su huggingface sono presenti diversi modelli BERT-like che potete usare, ovviamente, dato il task, è necessario usare modelli che "conoscono" la lingua italiana pertanto il BERT originario, che è solo in inglese, non lo possiamo utilizzare.
  
  Ecco alcuni modelli che potete usare: dbmdz/bert-base-italian-cased, m-polignano-uniba/bert_uncased_L-12_H-768_A-12_italian_alb3rt0, bert-base-multilingual-uncased, distilbert-base-multilingual-cased, xlm-roberta-base.

   ***! Attenzione !*** la lista non è esaustiva e alcuni modelli possono variare con versione cased/uncased, base/large, in particolare i modelli large non restituiscono un embedding di 768 elementi.

## Emoji

  Su python è possibile interagire con le emoji attraverso alcune librerie come [emoji](https://github.com/kyokomi/emoji) e [emot](https://github.com/NeelShah18/emot). Inoltre alcuni tokenizer dei modelli BERT-like hanno un token associato alle diverse emoji o, quando questo non è presente, è possibile aggiungere nuovi token e permettere al modello di apprendere una nuova rappresentazione tramite fine-tuning del modello. Questo si fa tramite la funzione add_tokens del tokenizer ed è necessario il resize del modello [more info + esempio](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizer.add_tokens)


Potete contattarmi via mail irene.siragusa02@unipa.it
