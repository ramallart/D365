# Què són les classes SysDa?

Les classes SysDa de Dynamics 365 són una interfície de programació d'aplicacions (API) que permet crear consultes extensibles.
L'API SysDa proporciona gairebé totes les possibilitats d'accés a dades que estan disponibles en X++.
De fet, les APIs són embolcalls al voltant del codi que generaria el compilador X++.

Hi ha un conjunt més petit de tipus que impulsa les activitats de consulta principals:
- **Select**: ```SysDaQueryObject```, ```SysDaSearchObject``` i ```SysDaSearchStatement```.
- **Update**: ```SysDaUpdateObject``` i ```SysDaUpdateStatement```.
- **Insert**: ```SysDaInsertObject``` i ```SysDaInsertStatement```.
- **Delete**: ```SysDaQueryObject```, ```SysDaDeleteObject``` i ```SysDaDeleteStatement```.

Aquestes classes es fan servir per executar les classes de SysDa que contenen consultes.
