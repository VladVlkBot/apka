<!DOCTYPE html>
<html>

<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
	<title>SDK_STROP</title>
	<link rel=stylesheet type=text/css href="style.css">
</head>

<body class="bodyFix">

	<h1>SDK strop rozpocet!</h1>

	<div class="_div_velky">
		<div class="_div_maly_ram">
			<label class="popis">VVEDTE:</label>
		</div>
		<div class="_div_maly"><input class="vvod" type="text" id="dilka" placeholder="DILKA ???" required>
		</div>
		<div class="_div_maly"><input class="vvod" type="text" id="sirka" placeholder="SIRKA ???" required>
		</div>
	</div>


	<div class="_div_velky">
		<div class="_div_maly_ram"><label class="popis">Zaklop:</label></div>
		<div class="_div_maly">
			<input id="radio_1" class="_radio_" type="radio" name="zaklop" value="1" checked>
			<label for="radio_1" class="labelRadio">1</label>
		</div>
		<div class="_div_maly">
			<input id="radio_2" class="_radio_" type="radio" name="zaklop" value="2">
			<label for="radio_2" class="labelRadio">2</label>
		</div>
	</div>

	<div class="_div_velky">
		<div class="_div_maly_ram"><label class="popis">Zavesy:</label></div>
		<div class="_div_maly">
			<input id="radio_3" class="_radio_" type="radio" name="zaves" value="primy" checked>
			<label for="radio_3" class="labelRadio">PRIMY</label>
		</div>
		<div class="_div_maly">
			<input id="radio_4" class="_radio_" type="radio" name="zaves" value="drat">
			<label for="radio_4" class="labelRadio">DRAT</label>
		</div>
	</div>

	<div class="_div_velky">
		<div class="_div_maly_ram"><label class="popis">Delka CD:</label></div>
		<div class="_div_maly">
			<input id="radio_5" class="_radio_" type="radio" name="metryCD" value="3" checked>
			<label for="radio_5" class="labelRadio">3m</label>
		</div>
		<div class="_div_maly">
			<input id="radio_6" class="_radio_" type="radio" name="metryCD" value="4">
			<label for="radio_6" class="labelRadio">4m</label>
		</div>
	</div>

	<div class="_div_velky">
		<div class="_div_maly_ram"><label class="popis">Vata:</label></div>
		<div class="_div_maly">
			<input id="radio_7" class="_radio_" type="radio" name="vata" value="ne" checked>
			<label for="radio_7" class="labelRadio">NE</label>
		</div>
		<div class="_div_maly">
			<input id="radio_8" class="_radio_" type="radio" name="vata" value="ano">
			<label for="radio_8" class="labelRadio">ANO</label>
		</div>
	</div>

	<div class="_div_velky">
		<div class="_div_maly_ram"><label class="popis">Folija:</label></div>
		<div class="_div_maly">
			<input id="radio_9" class="_radio_" type="radio" name="folija" value="ne" checked>
			<label for="radio_9" class="labelRadio">NE</label>
		</div>
		<div class="_div_maly">
			<input id="radio_10" class="_radio_" type="radio" name="folija" value="ano">
			<label for="radio_10" class="labelRadio">ANO</label>
		</div>
	</div>
	<br>
	<div class="_div_velky">
		<div class="_div_maly" style="cursor: pointer;" onclick="hlavni();"> POCITAT
		</div>
		<div class="_div_maly" style="cursor: pointer;" onclick="window.location.href = 'index.html';">
			HLAVNI
		</div>
	</div>


	<div id="_div">
		<h3 id="_h3" style="font-size: 35px; font-weight: 700;"></h3>
	</div>
	
	<script src="ceny.js"></script>

	<script>
		function hlavni() {
			
			h3 = document.getElementById('_h3')
			h3.style.color = 'red';

			let dilka = document.getElementById('dilka').value;
			let sirka = document.getElementById('sirka').value;
			let zaklop = document.getElementsByName('zaklop');
			let zaves = document.getElementsByName('zaves');
			let dilkaCD = document.getElementsByName('metryCD');
			let vata = document.getElementsByName('vata');

			for (let radio of zaklop) {
				if (radio.checked) {
					zaklop = radio.value;
				}
			}
			for (let radio of zaves) {
				if (radio.checked) {
					zaves = radio.value;
				}
			}
			for (let radio of dilkaCD) {
				if (radio.checked) {
					dilkaCD = Number(radio.value);
				}
			}
			for (let radio of vata) {
				if (radio.checked) {
					vata = radio.value;
				}
			}

			if (dilka == '' || sirka == '') {
				h3.textContent = 'Chibi data !!!'
			} else if (isNaN(Number(dilka)) || isNaN(Number(sirka))) {
				h3.textContent = 'Nekorektni data !!!'
			} else {
				var sl = pocitani(Number(dilka), Number(sirka), zaklop, zaves, dilkaCD, vata);
				vyvodSlovnyk(sl)
			}
		}

		function vyvodSlovnyk(slovnyk) {
			htmlKod = "<table class=\"text\">";
			for (const key in slovnyk) {
				if (slovnyk.hasOwnProperty(key)) {
					s = `<tr><td class=\"leva\">${key}</td><td class=\"centr\">${slovnyk[key][0]}</td><td class=\"prava\">${slovnyk[key][1]}</td></tr>`;
					htmlKod = htmlKod + s;
				}
			}
			htmlKod = htmlKod + '</table>' + '<br><div class=\"_div_maly\" style=\"cursor: pointer;\" onclick=\"window.location.href = \'SDK_STROP.html\';\">RESET</div>'
			document.body.innerHTML = htmlKod
		}

		function deleni(pocetRad, dilkaCD, dilkaRad, zbytekOk) {
			let spojky = 0;
			let cdMetry = 0;

			if (dilkaRad <= dilkaCD) {
				let n = parseInt(dilkaCD / dilkaRad);
				let z = dilkaCD - (dilkaRad * n);

				if (z < zbytekOk) {
					cdMetry = pocetRad / n * dilkaCD;
				} else {
					cdMetry = pocetRad * dilkaRad;
					spojky = pocetRad / (n + 1);
				}
			} else {
				cdMetry = pocetRad * dilkaRad;
				spojky = parseInt(dilkaRad / dilkaCD) * pocetRad;
			}
			return spojky, cdMetry;
		}
		
		function cenaKusyBaleni(cenaKus, cenaBaleni, pocetVBaleni, pocet){
			let baleni = 0;
			let kusy = 0;
			let cenaTemp = 0;
			let cenaZaKusy = pocet*cenaKus;
			if (cenaZaKusy >= cenaBaleni){
				baleni = parseInt(pocet/pocetVBaleni);
				let ostatok = pocet - (baleni*pocetVBaleni);
				if (ostatok > (pocetVBaleni-(pocetVBaleni*20/100))) {
					baleni += 1;
					cenaTemp = baleni*cenaBaleni;
					return [cenaTemp, baleni, kusy];
				}
				else {
					kusy = ostatok;
					cenaTemp = (baleni*cenaBaleni) + (kusy*cenaKus);
					return [cenaTemp, baleni, kusy];
				}
			}
			else if (cenaZaKusy > (cenaBaleni - (cenaBaleni*20/100))){
				baleni = 1;
				return [cenaBaleni, baleni, kusy];
			}
			else {
				return [pocet*cenaKus, baleni, pocet];
			}
		}
		
		function pocitani(dilka, sirka, zaklop, zaves, dilkaCD, vata) {
			const nosnePo = 0.85; /* ____________________________________________DILKA MEZI NOSNYMI ____________________*/
			const zavesyPo = 0.85;
			const dilkaUD = 3;
			const zbytekOk = 0.5;
			let sl = {};
			let cena = 0;
			let celkovaCena = 0;

			sl['DILKA POKOJE (m):'] = [dilka + ' m', ''];
			sl['SIRKA POKOJE (m):'] = [sirka + ' m', ''];

			/* Vypocet nosnych CD*/
			let spojkyNaNosne = 0;
			let cdNosneMetr = 0;
			let $pocetNosne = 0;

			if (sirka < 1.1) {
				$pocetNosne = 0;
			} else if (sirka >= 1.1 && sirka <= 1.4) {
				$pocetNosne = 1;
			} else {
				$pocetNosne = parseInt(sirka / nosnePo);
				let zbytek = sirka - ($pocetNosne * nosnePo);

				if (zbytek > 0.3) {
					$pocetNosne = $pocetNosne + 1;
				}
			}
			spojkyNaNosne, cdNosneMetr = deleni($pocetNosne, dilkaCD, dilka, zbytekOk);


			/*Vypocet montaznich CD*/
			let spojkyNaMontazne = 0;
			let cdMontazneMetr = 0;
			let $pocetMontazne = (Math.ceil(dilka / 0.5) + 1);
			spojkyNaMontazne, cdMontazneMetr = deleni($pocetMontazne, dilkaCD, sirka, zbytekOk);


			/*Vypocet spojky*/
			let spojky = spojkyNaNosne + spojkyNaMontazne;
			spojky = (spojky * 5 / 100) + spojky;

			/*Vypocet CD*/
			let $CD = cdNosneMetr + cdMontazneMetr;
			$CD = ($CD * 5 / 100) + $CD;
			$CD = Math.ceil($CD / dilkaCD);
			if (dilkaCD == 4) {cena = CENY['CD_4'];}
			else {cena = CENY['CD_3'];}
			sl[`CD prifil ${dilkaCD}m: <br>_____nosne: ${$pocetNosne} rad, <br>_____montazne: ${$pocetMontazne} rad`] = [$CD + ' ks', $CD*cena + ' Kc'];
			celkovaCena += $CD*cena;

			/*Vypocet UD*/
			let $UD = (dilka * 2) + (sirka * 2);
			$UD = ($UD * 5 / 100) + $UD;
			$UD = Math.ceil($UD / 3);
			cena = CENY['UD'];
			sl['UD profil 3m:'] = [$UD + ' ks', $UD*cena + ' Kc'];
			celkovaCena += $UD*cena;

			/*Vypocet SDK*/
			let $SDK = ((dilka) * (sirka)) / 2.5;
			$SDK = ($SDK * 15 / 100) + $SDK;
			$SDK = Math.ceil($SDK);
			if (zaklop == '2') {
				$SDK = $SDK * 2;
			}
			cena = CENY['SDK_BILA'];
			sl['SDK 1.25x2 :'] = [`${$SDK} ks`, $SDK*cena + ' Kc'];
			celkovaCena += $SDK*cena;

			/*Vypocet krizoveSpojky*/
			let krizoveSpojky = $pocetNosne * $pocetMontazne
			krizoveSpojky = Math.ceil((krizoveSpojky * 5 / 100) + krizoveSpojky);
			let arKS = cenaKusyBaleni(CENY['KRIZOVA_SPOJKA'], CENY['KRIZOVA_SPOJKA_100'], 100, krizoveSpojky);
			sl['Krizove spojky:<br>_____(baleni 100 ks)'] = [`${arKS[1]} bal<br>a ${arKS[2]} ks`, arKS[0] + ' Kc'];
			celkovaCena += cena;

			/*Vypocet zavesu*/
			let zavesyRad = parseInt((dilka - 0.4) / zavesyPo) + 1;
			zbytek = (dilka - 0.4) - (zavesyRad * zavesyPo);

			if (zbytek > 0.4) {
				zavesyRad = zavesyRad + 1;
			}
			let $zavesy = zavesyRad * $pocetNosne;

			if (zaves == 'drat') {
				sl['Nosny drat:'] = [$zavesy + ' ks', ''];
				sl['Zaves perovy:'] = [$zavesy + ' ks', ''];
			} else {
				let arPZ = cenaKusyBaleni(CENY['PRIMY_ZAVES_200'],CENY['PRIMY_ZAVES_200_100'],100,$zavesy);
				sl['Primy zaves:<br>_____(baleni 100 ks)'] = [`${arPZ[1]} bal<br>a ${arPZ[2]} ks`, arPZ[0] + ' Kc']
			}

			/*Vypocet hmozdinek betonjaku*/
			let bet = 0;
			if (zaves == 'drat') {
				bet = $zavesy;
			} else if (zaves == 'primy') {
				bet = $zavesy*2;
			}
			bet = Math.ceil(bet/100);
			sl['Hmozdinky betonjaky:<br>_____(baleni 100 ks)'] = [bet+' bal', CENY['BETONJAKY_100']*bet];

			/*Vypocet mozdinek do UD*/
			let hmozdinky = (sirka * 2 + dilka * 2) / 0.5;
			hmozdinky = Math.ceil((hmozdinky * 10 / 100) + hmozdinky);
			sl['Hmozdinky do UD:'] = [hmozdinky, ''];

			/*Vypocet sroubu*/
			let srouby = 0;
			if (zaklop == '1') {
				srouby = cdMontazneMetr / 0.2;
				srouby = Math.ceil((srouby * 10 / 100) + srouby);
				sl['Srouby 25mm:'] = [srouby, ''];
			} else if (zaklop == '2') {
				srouby = cdMontazneMetr / 0.4;
				sl['Srouby 25mm:'] = [srouby, ''];
				srouby = cdMontazneMetr / 0.2;
				srouby = Math.ceil((srouby * 10 / 100) + srouby);
				sl['Srouby 35mm:'] = [srouby, ''];
			}

			/*Vypocet vaty*/
			if (vata == 'ano') {
				sl['Vata:'] = [`${sirka*dilka} m2`, '']
			}
			sl['CELKOVA CENA: '] = ['', celkovaCena + ' Kc']
			return sl;
		}
	</script>
</body>

</html>
