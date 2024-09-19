MaxDatumTillgänglighet: 
LOAD
    Date(Max(TmpDatum), 'YYYY-MM-DD') as [SenasteDatumTillgänglighet]
//     Max(Monthend(TmpDatum)) as [SenasteDatumTillgänglighet]
RESIDENT 
    Fakta_Produktion_tmp
;

LET vSenasteDatumTillgänglighet = Peek('SenasteDatumTillgänglighet'); 
