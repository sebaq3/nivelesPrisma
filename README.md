# nivelesPrisma
mikrotik log fibertel rx tx




:local correrDerTX 28
:local correrDerRX 60
:local correrIzqRX 100
/tool fetch url="http://provisioning.fibertel.com.ar/asp/nivelesPrima.asp" mode=http src-path="/" dst-path="/niveles.txt"
:delay 2
:local result [/file get niveles.txt contents]
:local resultLen [:len $result]
:local startLoc [:find $result "valorDestacado" -1] 
:set startLoc [:tonum ($startLoc + $correrDerTX)] 
:local endLoc [:find $result "dBmV</td>" ]
:local tx [pick $result $startLoc $endLoc]
:log info "TX = $tx"
:local startLoc [:find $result "Rx</td>" -1]
:set startLoc [:tonum ($startLoc + $correrDerRX)]
:local endLoc [:find $result ">Freq Rx" ]
:set endLoc [:tonum ($endLoc - $correrIzqRX)]
:local rx [pick $result $startLoc $endLoc]
:log info "RX = $rx"
