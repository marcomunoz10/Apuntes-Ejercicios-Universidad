
CVE --> Identificador de una vulnerabilitat

Hi ha vulnerabilitats q estan associades a una fase en concret.

Estudi de viabilitat = feasibility stduy

Integer overflow --> Hay un contador que se desborda.


---
# Common Vulnerability Scoring System

Intenta definir quin risc té una vulnerabilitat. Organitza unes mètriques. Agafen tots els factors que pot influenciar una vulnerabilitat. 
1. Base Metrics: Hi ha aspectes q sempre seran iguals per diferents situacions.
	- Miren l'Exploitability Metrics.
		-Vector d'atac (des d'on es fa l'atac)
			- Network (fora)
			- Adjaçent (mateixa xarxa)
			- local (dintre del sistema)
			- Físicament allà
![[Pasted image 20250303115126.png]]
		-Attack complexity (si sempre funciona o si necessito q pasi alguna casuística q no depèn de l'atacant)
			- Low
			- High
		- Attack requeriments
		- Privileges required (privilegis de l'atacant en fer l'atac)
	- Miren el vulnerable system impact metrics.
	- Miren el subsequent System impacts metrics. --> altres sistemes a part de l'atacat son afectats. (exemple: la vulnerabilitat la presenta el fòrum amb el cross site scripting i puc possar codi que afecti al servidor)

2. Environmental Metric Group
- Si estic en una empresa aquestes mètriques només afectarán a aquella empresa. Permet modificar totes les mètriques del grup de mètriques base per adaptar-los a la meva empresa.
	Hi ha dos tipus:
	2.1 Modified Base Metrics
	2.2 Security Requirements
	
3. Threat Metric Group
	Exploit Maturity: Experiència d'eines automatitzades per fer l'atac.

