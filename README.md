# OmniLib

Modélisation UML du projet OmniLib - portail unique capable de gérer à la fois le stock physique historique et numérique de médias

## Diagramme de cas d'utilisation

### code du diagramme

```plantuml
@startuml

skinparam actorStyle awesome
left to right direction

together{
    actor "Visiteur" as Visiteur
    actor "Adhérant Standard" as AdhérantStandard
    actor "Adhérant Premium" as AdhérantPremium
}

actor "Bibliothécaire" as Bibliothécaire

rectangle "OmniLib"{
    usecase "Naviguer sur le portail" as Naviguer
    usecase "Gérer l'abonnement" as Gérer
    usecase "Emprunter un livre physique" as Emprunter
    usecase "Réserver un livre en ligne" as Réserver
    usecase "Se faire livrer à domicile" as Livraison
    usecase "Acheter" as Acheter
    usecase "Télécharger un E-book" as Télécharger
    usecase "Location de VOD" as Location
    usecase "Bloquer l'action" as Bloquer
    usecase "Mettre à jour le catalogue" as MettreAJour
    usecase "Valider les retours de livres" as Valider 
}

together{
    actor "Transporteur" as Transporteur
    actor "Banque" as Banque
}

Gérer -.-> Acheter : <<include>>
AdhérantStandard -left-|> Visiteur
AdhérantPremium -left-|> AdhérantStandard
Visiteur --- Naviguer
AdhérantStandard --- Emprunter
AdhérantStandard --- Réserver
AdhérantStandard --- Gérer
AdhérantPremium --- Télécharger
AdhérantPremium --- Location
Réserver <-.- Livraison : <<extends>>
Location <-.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Télécharger <-.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Emprunter <-.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Livraison <-right.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Livraison -left.-> Acheter : <<include>>
Acheter --- Transporteur
Acheter --- Banque
Bibliothécaire --- MettreAJour
Bibliothécaire --- Valider

@enduml
```

### rendu du diagramme

![usecase_diagram](./assets/usecase_diagram.png)
