# CrudCrud - API Consumer

## Nötige Applikationen

1. Visual Studio Code (recommended)
2. NodeJS LTS (https://nodejs.org/en/)
3. yarn

```
npm install --global yarn
```

## Projekt setup

Kann länger dauern, da hier das halbe Internet heruntergeladen wird (node modules).
Es ist wichtig das zuerst ins Project Directory gewechselt wird, also ins Verzeichnis ab16-4.

```
yarn install
```

### Programm als Dev Build ausführen

```
yarn serve
```

## User Manual

1. https://crudcrud.com/ öffnen
2. Endpoint kopieren (https://crudcrud.com/api/{endpoint})
3. Auf meine Webapplikation wechseln (läuft nach yarn serve normalerweise unter http://localhost:8080/)
4. Endpoint einfügen (falls Limit überschritten: Icognito Tab)
5. Über + neue Items hinzufügen
6. Über das Trash-Icon das Item aus der Tabelle löschen
7. Über das Pencil-Icon ein bestehendes Item in der Tabelle bearbeiten

`Achtung: Validierung und sauberes Error Handling wurde nicht implementiert!`
