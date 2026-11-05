# Les traders artificiels sont-ils surconfiants ?

**Analyse de la calibration des niveaux de confiance dans un système de trading multi-modèles LLM**

Nicolas Bourgeois — Master 2 IREN, Dauphine-PSL — Mars 2026

---

## Résumé

Ce projet étudie la **surconfiance probabiliste** des grands modèles de langage (LLM) lorsqu'ils sont déployés comme agents de trading autonomes. En s'appuyant sur les outils de la finance comportementale — notamment la taxonomie de Moore & Healy (2008) distinguant surestimation, sur-positionnement et sur-précision — l'étude évalue si les LLM reproduisent les biais cognitifs observés chez les traders humains.

**Question de recherche** : les agents de trading fondés sur des LLM sont-ils sujets à la surconfiance, et si oui, sous quelle forme ?

## Dispositif expérimental

Quatre modèles sont mis en concurrence sur une période de **72 heures** en environnement sandbox (paper trading) :

| Modèle | Fournisseur |
|---|---|
| GPT-4o-mini | OpenAI |
| LLaMA-3.3-70B | Meta (via Groq) |
| Gemini 1.5 Flash | Google DeepMind |
| Claude-3 Haiku | Anthropic |

Chaque agent gère un portefeuille virtuel de **100 EUR** sur un univers d'environ **450 instruments** (actions US, cryptomonnaies, ETFs). Les modèles reçoivent le même snapshot de marché à chaque cycle de décision (toutes les 10 minutes) et opèrent de manière strictement indépendante, sans interaction inter-agents.

La **confiance implicite** de chaque décision est capturée via l'inverse du score de risque déclaré par le modèle (`Confiance = 100 − score de risque`). La surconfiance est ensuite mesurée par l'**Expected Calibration Error (ECE)** :

$$ECE = \sum_{m=1}^{M} \frac{|B_m|}{n} \left| \text{acc}(B_m) - \text{conf}(B_m) \right|$$

## Résultats principaux

Sur **187 transactions** :

- **ECE globale : 0,2486** → miscalibration systémique confirmée
- Confiance moyenne déclarée : **81,9 %** vs. taux de succès réel : **61,5 %**
- Les agents surestiment systématiquement la probabilité de succès de leurs décisions

| Modèle | Win Rate | Confiance Moy. | ECE | Marge |
|---|---|---|---|---|
| Claude-3 Haiku | 90,0 % | 81,8 % | 0,1202 | +50 % |
| Gemini 1.5 Flash | 33,3 % | 86,2 % | 0,5289 | +700 % |
| GPT-4o-mini | 43,8 % | 77,9 % | 0,4419 | +20 % |
| LLaMA-3.3-70B | 100 %* | 73,7 % | 0,2626 | +20 % |

*\* Échantillon réduit (23 trades)*

**Résultat notable** : Gemini affiche la pire calibration (ECE = 0,53) mais le meilleur profit (+700 %), illustrant que mauvaise calibration ≠ mauvaise rentabilité.

## Données

Le fichier [`export_calibration_study.csv`](export_calibration_study.csv) contient l'intégralité des 187 transactions avec :
- Modèle (agent)
- Confiance implicite déclarée
- Résultat (win/loss)
- P&L

> **Note** : le code source du système de trading n'est pas publié. Le volume de développement est considérable et la diffusion d'un outil de trading autonome opérationnel soulèverait des risques éthiques disproportionnés.

## Références clés

- Moore, D. A. & Healy, P. J. (2008). *The trouble with overconfidence*. Psychological Review.
- Glaser, M. & Weber, M. (2007). *Overconfidence and trading volume*. Geneva Risk and Insurance Review.
- Lopez-Lira, A. & Tang, Y. (2025). *Can ChatGPT Forecast Stock Price Movements?* Working Paper.
- OpenAI (2023). *GPT-4 Technical Report*. arXiv:2303.08774.
- Lin, S., Hilton, J. & Evans, O. (2022). *Teaching models to express their uncertainty in words*. arXiv:2205.14334.
- Dorst, K. (2023). [*ChatGPT undermines the case for human irrationality*](https://kevindorst.substack.com/p/chatgpt-undermines-the-case-for-human). Substack.

## Licence

Ce dataset est mis à disposition à des fins académiques et de recherche.
