# OmniLib

Modélisation UML du projet OmniLib - portail unique capable de gérer à la fois le stock physique historique et numérique de médias

## Diagramme de cas d'utilisation

### Code du diagramme

```plantuml
@startuml

skinparam actorStyle awesome
left to right direction

together{
    actor "Visiteur" as Visiteur
    actor "Adhérant Standard" as AdherantStandard
    actor "Adhérant Premium" as AdherantPremium
}

actor "Bibliothécaire" as Bibliothecaire

rectangle "OmniLib"{
    usecase "S'identifier" as Identifier
    usecase "Naviguer sur le portail" as Naviguer
    usecase "Gérer l'abonnement" as Gerer
    usecase "Emprunter un livre physique" as Emprunter
    usecase "Réserver un livre en ligne" as Reserver
    usecase "Se faire livrer à domicile" as Livraison
    usecase "Acheter" as Acheter
    usecase "Télécharger un E-book" as Telecharger
    usecase "Location de VOD" as Location
    usecase "Bloquer l'action" as Bloquer
    usecase "Mettre à jour le catalogue" as MettreAJour
    usecase "Valider les retours de livres" as Valider 
}

together{
    actor "Transporteur" as Transporteur
    actor "Banque" as Banque
}

Visiteur --- Identifier
Gerer -[hidden]right-> Reserver
Gerer -.-> Acheter : <<include>>
AdherantStandard -left-|> Visiteur
AdherantPremium -left-|> AdherantStandard
Visiteur --- Naviguer
AdherantStandard --- Emprunter
AdherantStandard --- Reserver
AdherantStandard --- Gerer
AdherantPremium --- Telecharger
AdherantPremium --- Location
Reserver <-.- Livraison : <<extends>>
Location <-.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Telecharger <-.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Emprunter <-.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Livraison <-right.- Bloquer : <<extends>>\n condition:{pénalité de retard}
Livraison -left.-> Acheter : <<include>>
Acheter --- Transporteur
Acheter --- Banque
Bibliothecaire --- MettreAJour
Bibliothecaire --- Valider

@enduml
```

### rendu du diagramme de cas d'utilisation

![usecase_diagram](./assets/usecase_diagram.png)

## Diagramme d'activités

### Code du diagramme d'activités pour réserver un livre en ligne

```plantuml
@startuml
start
repeat
    :Tentative de connexion;
repeat while (Connexion réussie) is (non) not (oui)
:Réserver un livre;
if(L'adhérent à une pénalité de retard) then (oui)
    :processus bloqué;
    stop
else(non)
    if(Livraison à domicile) then (oui)
        :Payer 5 euros; 
    else(non)
    endif
    :Commande du livre;
endif
stop

@enduml
```

### rendu du diagramme d'activités

![usecase_diagram](./assets/activities_diagram.png)

## Diagramme de classes

```mermaid
classDiagram
    Livre --|> ArticlePhysique
    Article <|-- ArticlePhysique
    Article <|-- ArticleNumerique
    ArticleNumerique <|-- EBook
    ArticleNumerique <|-- VOD
    Personne <|-- Bibliothecaire
    Adherent --|> Personne
    AdherentPremium --|> Adherent
    Bibliotheque o-- Personne
    Bibliotheque o-- Article
    ArticlePhysique o-- Personne : emprunteur (1) 
    ArticleNumerique o-- Personne : emprunteurs (*) 

    class Bibliotheque{
        -String name
        +emprunterLivre(Livre l, Adherent a)
        +reserverLivre(Livre l, Adherent a)
        +bloquerAdherant(Adherent a)
        +livrerDomicile(Adherent a)
        +telechargerEBook(EBook e, AdherentPremium a)
        +louerVOD(VOD v, AdherentPremium a)
        +mettreAJour(Livre l, Bibliotecaire b)
        +validerRetour(Livre l, Bibliotecaire b)

    }

    class Personne {
        <<Abstract>>
        -String nom
        -String motDePasse
        -int id
        +getNom() String
        +getId() int
    }
    class Adherent{
        -boolean bloque
        -String adresse
        -boolean premium
        +getBloque() boolean
        +isPremium()

    }
    class AdherentPremium{
        -int dateExpiration
        +getDateExîration()
    }
    class Bibliothecaire{
        -int codeEmployee
        +getCodeEmployee()
    }
    class Article{
        <<Abstract>>
        -String titre
        -String auteur
        -int dateParution
        +getTitre() String
        +getAuteur() String
        +getDateParution() int
    }

    class ArticlePhysique{
        <<Abstract>>
        +getAdherent()
    }
    class ArticleNumerique{
        <<Abstract>>
        +getListAdherents()
    }
    class Livre{
        -int codeBarre
        -String rayon
        -String usure
        +getCodeBarre() int
        +getRayon() String
        +getUsure() String
    }
    class EBook{
        -int taille
        -String format
        +getTaille() int
        +getFormat() String
    }
    class VOD{
        -int duree
        -int resolution
        +getDuree() int
        +getResolution() int
    }

```
