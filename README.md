# RET
repo electrical til
SELECT
a.mrbtsId,lnBtsId,lnCelId,siteId,siteCId,
a.eqmId,a.apeqmId,a.aldId,a.retUId,angle,sectorID
FROM
(SELECT
mrbtsId,lnBtsId,lnCelId,siteId,siteCId,
eqmId,apeqmId,aldId,retUId
FROM
(SELECT
mrbtsId,lnBtsId,lnCelId,siteId,siteCId
FROM a_lte_mrbts_lnbts_lncel)a
INNER JOIN
(SELECT
targetCellDN,eqmId,apeqmId,aldId,retUId,
SUBSTRING_INDEX(SUBSTRING_INDEX(targetCellDN,"/",2),"-",-1) AS MRBTS,
SUBSTRING_INDEX(SUBSTRING_INDEX(targetCellDN,"/",3),"-",-1) AS LNBTS,
SUBSTRING_INDEX(SUBSTRING_INDEX(targetCellDN,"/",4),"-",-1) AS LNCEL
FROM a_eqm_apeqm_ald_retu_crel)b on a.mrbtsId = b.MRBTS AND a.lnBtsId = b.LNBTS AND a.lnCelId = b.LNCEL
)a
INNER JOIN
(SELECT
mrbtsId,eqmId,apeqmId,aldId,retUId,angle,sectorID
FROM a_eqm_eqm_apeqm_ald_retu)b ON a.mrbtsId = b.mrbtsId AND a.eqmId = b.eqmId AND a.aldId = b.aldId AND a.retUId = b.retUId;
