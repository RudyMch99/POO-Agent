# POO-Agent
Cours POO et C++


Exercice 1 : Spécial agent utilisant le patron State

Le SpecialAgent Hérite des classes Agent et StateMachine, StateMachine compose de State qui elle meme hérite des états ScanState et AttackState.

```mermaid

classDiagram
    State o--*  StateMachine : compose \n\n agrège
    State <|--  ScanState : hérite de
    State <|--  AttackState : hérite de
    Agent <|-- SpecialAgent : hérite de
    StateMachine <|--  SpecialAgent : hérite de


    class SpecialAgent{
        #String name
        #int x
        #int y
        #int orientation
        #int life
        #int ammo
        #int distance
        +update() void  
        #onUpdate() void
    }
    class Agent{
        #String name
        #int x
        #int y
        #int orientation
        #int life
        #int ammo
        #int distance
        +update() void
        #onUpdate() void
    }
    class StateMachine{
        -actualState : State
        + setState(in : State) void
        + doAction() void
    }

    class State{
        #stateMachine : StateMachine
        +doAction() void
    }

    class ScanState{
        #String color
        +doAction() void
    }

    class AttackState{
        #String color
        +doAction() void
    }

```

ScanState est l'état initiale de l'agent (couleur verte, se déplace et s’oriente de manière pseudo-aléatoire) et passe en AttackState (couleur rouge, se déplace vers ennemi et lui tire dessus) si un ennemi est en vue.

```mermaid
stateDiagram-v2
    [*] --> StatScan
    StatScan --> StateAttack : ennemi en vue
    StateAttack --> StatScan : ennemi perdu de vue

```

Ce diagramme de séquence montre les étapes d'un programme informatique qui crée un agent spécial appelé "Henry", puis le rafraîchit. L'agent utilise ensuite une machine d'état qui passe à l'état "Scan" et effectue une action. Ensuite, l'état change pour "Attack". 

```mermaid
sequenceDiagram
    activate main
    main ->> SpecialAgent : new SpecialAgent("Henry")
    activate SpecialAgent
    main->>SpecialAgent: refresh()
    SpecialAgent ->> StateMachine : new StateMachine(New ScanState)
     activate StateMachine
     activate ScanState
    SpecialAgent->>StateMachine: update()
    StateMachine->>ScanState: doAction()
    ScanState->>StateMachine: setState(new AttackState())
    activate AttackState
    StateMachine -x ScanState: delete
     deactivate ScanState
    SpecialAgent ->> StateMachine: delete
    StateMachine -x AttackState: delete
     deactivate AttackState
    StateMachine -x StateMachine: ~StateMachine
    deactivate StateMachine
    main -x SpecialAgent: delete
    deactivate SpecialAgent
    deactivate main


```