# heartbeat
Roll20's Template for Hearts

## Notes for translation.json
- the ***language*** property is necessary for a internal function ( to sort string ).
  It shall correspond to the two-character coding of ISO 639-1
- replace the ***translation.json*** file with the one of the chosen language in the ***translation*** 

## Règles CSS
Compilation des règles CSS utilisable pour la mise en page.

- disposition flex
  flex-row
  flex-col flex-col2 flex-col3 ... flex-col11
  flex-col-auto
- input-group
    input-group
    input-group-prepend input-group-append input-group-text
- accordéon
    accordion
    accordion-checkbox accordion-label
    accordion-container
- style générique
    center
    strong
    bold
    alert
- input
    fixed-small fixed-medium fixed-large
    
## Remarques sur roll20 et la création de la fiche de personnage
quelques "pense-bête" pour certains aspects pas évident à deviner lors de la création de la fiche de personnage.

- Debug
    [git alpha](https://raw.githubusercontent.com/aloade/pathfinder/master/image/)
    
    [git dev.](https://raw.githubusercontent.com/aloade/roll20-character-sheets/master/Pathfinder%20Adventurer%27s/image/)
    
    [git prod.](https://raw.githubusercontent.com/Roll20/roll20-character-sheets/master/Pathfinder%20Adventurer%27s/image/)

- HTML
    - **ne pas** utiliser le terme **_max** ou **_maximum** pour les variables utilisés dans des calculs; des comportements aléatoires sont à prévoir ( par exemple "@{hitpoints_max}" renvoie toujours 0 )
    - pour un "radio" les inputs **doivent** se suivre dans le code et **doivent** avoir l'attribute value
    - les balises html5 dans leur majorité ne sont pas autorisés
    - les attributes "data" pour les balises sont supprimés
    - pour les fieldset "repeating_xxx" ne pas utliser les undescore pour le nommage de la classe
- ECMAscript

- CSS
    - les règles pour "rolltemplate" sont indépendants du "character sheet"
    - les input ont la règle "width" trop restrictif; obligation d'utiliser "important" pour appliquer un style personnalisé
    - les règles sur "html" sont ignorées, donc au revoir les tailles en "rem"
    - les images en base64 ne peuvent être intégrés dans les styles CSS

- SheetWorker
    - roll20 ne gère pas les négatifs de négatifs, pour gérer les négatifs on doit utiliser ***-(@{variable})***
    - les champs sont pensés **uniquement** pour les nombres, (disabled="disabled", type="hidden", value=@{[...]}, active ces fonctions ).
    
    pour travailler sur des string il est **obligatoire** d'utiliser un type "text" et l'attribut "readonly"
    - si des repeating sont en cause, les résultats des calculs doivent être envoyés vers des input "hidden"
      ( quand l'attribut "disabled" est présent les calculs sont 'parasités' )
    - getAttr renvoie l'attribut "value" brut
      ( la valeur n'est pas calculé à la volée et renvoi un string brut )
    - si un input avec l'attribut "disabled" a un calcul incluant un négatif d'un négatif, le résultat échoue silencieusement ?!
    - les bouton de type "action" ne doivent pas contenir d'underscore.
- champ autocalc
    - pour afficher une valeur à zéro ou un nombre donné, avec une entrée à 0 ou 1 ( checkbox de roll20 par exemple ), utiliser le calcul suivant :
      > x * ( @{attribut} + 1 - abs( @{ attribut } - 1 ) ) / 2
      
      où "x" est la valeur souhaitée si non zéro.
      
- translation.json
    - le message d'erreur "Foudn a pre-defined key order!" correspond à une liste d'élément ordonné contenant une erreur.
- rollTemplate
    - pas de calculs conditionnels utilisable, uniquement de l'affichage
      par exemple pour s'assurer qu'une valeur est au minimum à 1, utiliser :
      > /roll { 1d1, { 1d20+@{attribut} } }dl1
      
      ou pour avoir une valeur maximale à 20, utiliser :
      > /roll { 1d0+20, { 1d20+@{attribut} } }kh1 
    - règles pour les inégalités
        - x < y :
          > {{#rollLess() x y }} ... {{/rollLess() x y }}
        - x <= y :
          > {{#^rollGreater() x y }} ... {{/^rollGreater() x y }}
        - x = y :
          > {{#rollBetween() x y y }} ... {{/rollBetween() x y y }} 
        - x > y :
          > {{#rollGreater() x y }} ... {{/rollGreater() x y }}
        - x >= y :
          > {{#^rollLess() x y }} ... {{/^rollLess() x y }}
