
### **SPAM**

1. **Spam Filters vs. Scams Dirigits**:
    
    - Els filtres de correu brossa són efectius contra spam massiu però ineficients contra atacs dirigits (com ara el Business Email Compromise, BEC).
    - Els scams dirigits solen utilitzar emails que semblen legítims i bypassen els filtres tradicionals.
2. **Tipus de Scams**:
    
    - **BEC/CEO Fraud**: Els atacants es fan passar per alts càrrecs d'una empresa per sol·licitar transferències de diners.
    - **Inheritance Scam**: Es fingeix una herència que necessita una resposta urgent per no perdre’s.
    - **Stranded Traveler Scam**: Pretén que algú proper està en problemes a l'estranger.
3. **Tècniques d’Engany**:
    
    - **Email Spoofing**: Suplantar l'adreça d'un remitent conegut o fiable.
    - **Dominis Similars**: Utilització de dominis que semblen legítims amb diferències subtils (p. ex., “preciseaccontingservices” en lloc de “preciseaccountingservices”).
    - **Històries Plausibles**: Ús d'elements com la urgència, sumes grans de diners, o tragèdies per convèncer.
4. **Defenses i Prevenció**:
    
    - **DMARC**: Protocol per verificar la identitat dels remitents.
    - **Algoritmes d’Intel·ligència Artificial**: Detecció d’elements sospitosos com paraules clau o dominis enganyosos.
    - **Educació i Conscienciació**: Coneixement sobre com identificar correus sospitosos.
5. **Impacte**:
    
    - Les pèrdues financeres són molt elevades, especialment per als grups més vulnerables.
    - Les empreses també són afectades, i els seus costos sovint repercuteixen en els consumidors.


### **FingerPrinting**

1. **Cookies i Seguiment Tradicional**:
    
    - Inicialment, es feien servir galetes (_cookies_) per emmagatzemar dades del navegador i identificar usuaris retornats.
    - Les galetes de tercers permetien que companyies de publicitat rastregessin els usuaris a través de múltiples llocs web, generant perfils detallats.
2. **Empremta Digital del Navegador**:
    
    - Una tècnica més avançada que identifica usuaris recollint informació sobre les característiques úniques del dispositiu, com la resolució de la pantalla, els complements instal·lats o les fonts del sistema.
    - Aquesta tècnica funciona fins i tot quan les galetes són bloquejades o eliminades.
3. **Problemes de Privacitat**:
    
    - Les dades recollides poden revelar informació personal sensible, com preferències, condicions mèdiques o interessos polítics.
    - Molts usuaris són inconscients del seguiment i tenen poc control sobre aquestes tècniques.
4. **Respostes dels Usuaris**:
    
    - S'han desenvolupat extensions i eines per intentar protegir la privacitat, com ara _AdBlock Plus_, _Ghostery_ i navegadors en mode privat.
    - Aquestes eines no sempre són eficaces, ja que les tècniques de _fingerprinting_ evolucionen constantment.
5. **Possibles Solucions**:
    
    - Crear navegadors en núvol que uniformitzin les empremtes digitals.
    - Llistes negres per bloquejar scripts de _fingerprinting_.
    - Regulacions més estrictes per protegir la privacitat dels usuaris.

### **ComputationalPropaganda**

#### **Empremta Digital del Navegador**

- **Seguiment per Cookies**: Utilitzades inicialment per recordar informació de l'usuari, com sessions o preferències. Les galetes de tercers van permetre a empreses de publicitat rastrejar usuaris en múltiples llocs web.
- **Empremta Digital**: Una tècnica per identificar dispositius únics utilitzant característiques com la resolució de pantalla, fonts instal·lades, complements del navegador, etc.
- **Impacte en la Privacitat**: Permet identificar usuaris fins i tot en mode privat o amb galetes bloquejades.
- **Solucions Proposades**: Extensions de navegadors, navegadors en núvol o regulacions més estrictes per protegir la privacitat.

#### **Propaganda Computacional**

- **Ús de Bots en Xarxes Socials**: Bots automatitzats que promouen candidats polítics, desinformació o continguts virals.
- **Casos Estudiats**: Eleccions als EUA (2016), el Brexit, i campanyes a Brasil.
- **Estratègies**:
    - **Astroturfing**: Crear falses impressions de suport massiu.
    - **Hijacking de Hashtags**: Apropiar-se de hashtags d'opositors.
    - **Retweet Storms**: Retweets massius per amplificar missatges.
- **Problemes**:
    - Desinformació massiva.
    - Manipulació d'eleccions amb eines d'anàlisi de dades i bots.
- **Solucions Proposades**: Política pública per regular l'ús de bots, investigació independent i millores en seguretat per part de les plataformes.



#### **IPFS**
1. **Què és IPFS?**
    
    - És un sistema distribuït de fitxers que funciona amb una xarxa **peer-to-peer (P2P)**.
    - Substitueix el model tradicional d'adreçament IP per **adreçament de contingut**: es demana un fitxer concret en lloc d'un servidor específic.
2. **Com Funciona?**
    
    - Utilitza identificadors únics (hash) per accedir a dades específiques.
    - Els dispositius actuen com a clients i servidors, compartint i rebent fitxers directament.
3. **Avantatges del Model P2P i IPFS**:
    
    - Reducció del tràfic centralitzat i millor eficiència.
    - Major seguretat gràcies a la verificació de contingut immutable.
    - Resistència a censura i interrupcions del servidor original.
    - Operativa fins i tot en xarxes amb connectivitat intermitent.
4. **Aplicacions i Casos d'Ús**:
    
    - Compartició segura de dades científiques, suport a blockchains, e-commerce, i xarxes socials alternatives.
    - Mirroring de contingut censurat, com Wikipedia a Turquia o contingut del referèndum de Catalunya.
5. **Limitacions i Reptes**:
    
    - Les URL són poc amigables (basades en hash).
    - Dependència del comportament dels nodes (poden no estar sempre en línia).
    - Dificultat per integrar noms llegibles i generalitzar l'ús per a usuaris no tècnics.
