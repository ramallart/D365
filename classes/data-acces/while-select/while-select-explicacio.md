# Com funciona el while select?

És la forma més utilitzada del statement ```select``` en X++. El ```while select``` recorre molts registres (que compleixin certs criteris definits per l'usuari) i pot executar una declaració en cada registre.

Cal tenir en compte que en un ```while select```, el ```select``` es fa únicament un cop (just abans de la iteració) i que, qualsevol expressió booleana afigida es prova només una vegada.

Els resultats d'un statement ```while select``` es retornen en una variable de buffer de taula. Si s'utilitza una llista de camps dins del statement, només estaran disponibles aquests camps dins de la variable de taula. Si s'utilitza funcions agregades, els resultats es retornen en els camps sobre els qual es realitza l'agregació.
