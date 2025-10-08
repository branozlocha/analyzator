import React, { useState } from 'react';
import { TrendingUp, AlertCircle, CheckCircle, DollarSign, MapPin } from 'lucide-react';

export default function RealtyAnalyzer() {
  const [formData, setFormData] = useState({
    cena: '',
    pocet_izieb: '',
    lokalita: 'Bratislava - Stare mesto',
    ulica: '',
    plocha: '',
    vlastnictvo: '',
    poschodie: '',
    stav: '',
    popis: ''
  });
  const [analysis, setAnalysis] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState('');
  const [showMap, setShowMap] = useState(false);

  // Database of streets in Staré Mesto with detailed info
  const streetDatabase = {
    'panská': {
      name: 'Panská',
      lat: 48.1440,
      lon: 17.1082,
      info: `Panská ulica je jedna z najvýznamnejších a najprestížnejších ulíc v historickom centre Bratislavy.

**Poloha a charakter:**
- Nachádza sa v srdci Starého Mesta, paralelne s Hlavným námestím
- Historická ulica s reprezentatívnymi budovami a obchodmi
- Pešia zóna s vysokou koncentráciou obchodov a služieb

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (300m, 4 min chôdze)
- MŠ Grösslingova 23 (450m, 6 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (600m, 8 min chôdze)
- ZŠ Záborského 1 (850m, 11 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (250m, 3 min chôdze)
- Billa Obchodná 68 (400m, 5 min chôdze)
- Lekáreň Dr. Max, Panská 27 (priamo v ulici)
- Lekáreň Salvator, Hlavné nám. 8 (150m)
- DM Drogéria, Obchodná (350m)

**Zdravotné zariadenia:**
- Poliklinika Ružinov (1.2km, 15 min MHD)
- Všeobecná ambulancia Dr. Kováč, Panská 15
- Detský lekár MUDr. Nováková, Ventúrska 3 (200m)

**MHD:**
- Zastávka Hodžovo námestie (250m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zochova (400m) - linky 93, 203, 207

**Parky a oddych:**
- Hviezdoslavovo námestie (400m, 5 min chôdze)
- Sad Janka Kráľa (800m, 10 min chôdze)
- Medická záhrada (600m, 8 min chôdze)

**Reštaurácie a kaviarne:**
- Zylinder Café, Panská 18
- Mondieu, Panská 10
- Urban House, Laurinská 16 (150m)
- Pulitzer Café, Ventúrska (200m)

**Kultúrne zariadenia:**
- Slovenské národné divadlo (400m)
- Historické múzeum (300m)
- Primaciálny palác (250m)`
    },
    'michalská': {
      name: 'Michalská',
      lat: 48.1450,
      lon: 17.1070,
      info: `Michalská ulica je historická ulica v centre Starého Mesta, vedúca od Michalskej brány.

**Poloha a charakter:**
- Historická ulica s unikátnou atmosférou
- Úzka stredoveká ulička s Michalskou vežou
- Koncentrácia reštaurácií, kaviarní a obchodov so suvenírmi

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (250m, 3 min chôdze)
- MŠ Beblavého 1 (400m, 5 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (700m, 9 min chôdze)
- ZŠ Komenského nám. 6 (900m, 12 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (300m, 4 min chôdze)
- Billa Obchodná (500m, 7 min chôdze)
- Lekáreň Pod vežou, Michalská 8 (priamo v ulici)
- Lekáreň U Michala, Ventúrska 1 (100m)
- DM Drogéria Obchodná (450m)

**Zdravotné zariadenia:**
- Všeobecná ambulancia, Ventúrska 10 (150m)
- Stomatológia Dr. Horváth, Michalská 5
- Detský lekár, Kapitulská 8 (200m)

**MHD:**
- Zastávka Hodžovo námestie (300m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zochova (350m) - linky 93, 203, 207

**Parky a oddych:**
- Hviezdoslavovo námestie (350m, 5 min chôdze)
- Prezidentská záhrada (700m, 9 min chôdze)
- Medická záhrada (550m, 7 min chôdze)

**Reštaurácie a kaviarne:**
- Slovak Pub, Obchodná 62 (200m)
- Flag Ship Restaurant, Michalská 1
- Café Verne, Hviezdoslavovo nám.
- Štúr Café, Michalská 10

**Kultúrne zariadenia:**
- Múzeum zbraní v Michalskej veži
- Slovenské národné divadlo (500m)
- Galéria mesta Bratislavy (300m)`
    },
    'ventúrska': {
      name: 'Ventúrska',
      lat: 48.1445,
      lon: 17.1075,
      info: `Ventúrska ulica je kľúčová ulica Starého Mesta spájajúca Hlavné námestie s okolitými časťami.

**Poloha a charakter:**
- Paralelná s Michalskou ulicou
- Živá ulica s reštauráciami a obchodmi
- Časť pešej zóny

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (200m, 3 min chôdze)
- MŠ Beblavého 1 (350m, 5 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (650m, 8 min chôdze)
- ZŠ Záborského 1 (800m, 10 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (200m, 3 min chôdze)
- Billa Obchodná (450m, 6 min chôdze)
- Lekáreň Ventúrska, Ventúrska 10 (priamo v ulici)
- Lekáreň Dr. Max Panská (150m)
- Rossmann Obchodná (400m)

**Zdravotné zariadenia:**
- Všeobecná ambulancia Dr. Kováč, Ventúrska 10
- Stomatológia, Panská 20 (100m)
- Detský lekár MUDr. Nováková, Ventúrska 3

**MHD:**
- Zastávka Hodžovo námestie (200m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zochova (300m) - linky 93, 203, 207

**Parky a oddych:**
- Hviezdoslavovo námestie (300m, 4 min chôdze)
- Sad Janka Kráľa (750m, 10 min chôdze)
- Medická záhrada (500m, 7 min chôdze)

**Reštaurácie a kaviarne:**
- Pulitzer Café, Ventúrska 5
- Brasserie Stará Tržnica, Ventúrska
- Modra Hviezda, Beblavého 14
- Panská Mäsiarňa, Panská 17 (100m)

**Kultúrne zariadenia:**
- Primaciálny palác (200m)
- Slovenské národné divadlo (450m)
- Stará radnica (150m)`
    },
    'zámocká': {
      name: 'Zámocká',
      lat: 48.1430,
      lon: 17.1050,
      info: `Zámocká ulica spája centrum Starého Mesta s Bratislavským hradom.

**Poloha a charakter:**
- Svah vedúci k Bratislavskému hradu
- Tichšia ulica s historickými budovami
- Mix obytných domov a administratívnych budov

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (500m, 7 min chôdze)
- MŠ Mudroňova 2 (600m, 8 min chôdze)

**Základné školy (do 1km):**
- ZŠ Komenského nám. 6 (700m, 9 min chôdze)
- ZŠ Grösslingova 4 (850m, 11 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (500m, 7 min chôdze)
- Fresh Market, Beblavého (400m, 5 min chôdze)
- Lekáreň Salvator, Panenská 8 (300m)
- Lekáreň Pod Hradom, Zámocká 20 (200m)

**Zdravotné zariadenia:**
- Poliklinika Staré Mesto, Bezručova 7 (600m)
- Všeobecná ambulancia, Beblavého 10 (350m)

**MHD:**
- Zastávka Zámocká (100m) - linky 203, 207, 208
- Zastávka Hodžovo námestie (400m) - linky 201, 203, 205, 206

**Parky a oddych:**
- Hradné sady (300m, priamy prístup)
- Park pri Bratislavskom hrade (400m)
- Horský park (800m)

**Reštaurácie a kaviarne:**
- UFO Watch.Taste.Groove (800m)
- Hradná viecha (350m)
- Leberfinger, Beblavého 8 (300m)

**Kultúrne zariadenia:**
- Bratislavský hrad (500m)
- Historické múzeum v hrade
- Dom U dobrého pastiera (250m)`
    },
    'hlavné námestie': {
      name: 'Hlavné námestie',
      lat: 48.1435,
      lon: 17.1077,
      info: `Hlavné námestie je centrálne námestie a srdce Starého Mesta Bratislavy.

**Poloha a charakter:**
- Centrálne námestie Bratislavy
- Historické budovy, Stará radnica, fontána Rolanda
- Plně pešia zóna

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (350m, 5 min chôdze)
- MŠ Beblavého 1 (300m, 4 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (550m, 7 min chôdze)
- ZŠ Komenského nám. 6 (750m, 10 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (300m, 4 min chôdze)
- Billa Obchodná (350m, 5 min chôdze)
- Lekáreň Salvator, Hlavné nám. 8 (priamo na námestí)
- Lekáreň Dr. Max, Panská (100m)
- DM Drogéria, Obchodná (300m)

**Zdravotné zariadenia:**
- Všeobecná ambulancia, Ventúrska 10 (150m)
- Stomatológia, Panská 15 (100m)
- Detský lekár, Kapitulská 8 (300m)

**MHD:**
- Zastávka Hodžovo námestie (200m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zochova (350m) - linky 93, 203, 207

**Parky a oddych:**
- Hviezdoslavovo námestie (200m, 3 min chôdze)
- Sad Janka Kráľa (700m, 9 min chôdze)
- Medická záhrada (500m, 7 min chôdze)

**Reštaurácie a kaviarne:**
- Café Roland, Hlavné nám. 5
- Prešporáčik, Hlavné nám. 12
- Verne Urban Garden, Hviezdoslavovo nám.
- Sky Bar & Restaurant, Hviezdoslavovo nám.

**Kultúrne zariadenia:**
- Mestské múzeum v Starej radnici (priamo na námestí)
- Primaciálny palác (150m)
- Slovenské národné divadlo (300m)`
    },
    'obchodná': {
      name: 'Obchodná',
      lat: 48.1458,
      lon: 17.1090,
      info: `Obchodná ulica je najdôležitejšia obchodná tepna historického centra Bratislavy.

**Poloha a charakter:**
- Hlavná nákupná ulica Bratislavy
- Pešia zóna s množstvom obchodov a služieb
- Vysoká frekvencia chodcov

**Materské školy (do 500m):**
- MŠ Grösslingova 23 (300m, 4 min chôdze)
- MŠ Mudroňova 2 (450m, 6 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (400m, 5 min chôdze)
- ZŠ Záborského 1 (700m, 9 min chôdze)

**Supermarkety a obchody:**
- Billa Obchodná 68 (priamo v ulici)
- Tesco Kamenné námestie (200m, 3 min chôdze)
- DM Drogéria, Obchodná 62 (priamo v ulici)
- Rossmann, Obchodná 50 (priamo v ulici)
- Lekáreň Obchodná, Obchodná 45 (priamo v ulici)

**Zdravotné zariadenia:**
- Poliklinika Ružinov (1.5km, 20 min MHD)
- Všeobecná ambulancia Dr. Varga, Grösslingova 8 (200m)
- Stomatológia Smile, Obchodná 52

**MHD:**
- Zastávka Hodžovo námestie (150m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Poštová (250m) - linky 93, 203

**Parky a oddych:**
- Hviezdoslavovo námestie (300m, 4 min chôdze)
- Medická záhrada (400m, 5 min chôdze)
- Sad Janka Kráľa (900m)

**Reštaurácie a kaviarne:**
- Slovak Pub, Obchodná 62
- McDonald's, Obchodná
- KFC, Obchodná
- Starbucks, Obchodná 58
- Costa Coffee, Obchodná

**Kultúrne zariadenia:**
- Slovenské národné divadlo (350m)
- Reduta (200m)
- Galéria Nedbalka (100m)`
    },
    'laurinská': {
      name: 'Laurinská',
      lat: 48.1442,
      lon: 17.1088,
      info: `Laurinská ulica je elegantná ulica v centre Starého Mesta.

**Poloha a charakter:**
- Paralelná s Obchodnou ulicou
- Tichšia alternatíva k rušnej Obchodnej
- Mix obchodov, reštaurácií a bytov

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (250m, 3 min chôdze)
- MŠ Grösslingova 23 (350m, 5 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (500m, 7 min chôdze)
- ZŠ Záborského 1 (750m, 10 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (200m, 3 min chôdze)
- Billa Obchodná (250m, 3 min chôdze)
- Lekáreň Dr. Max, Panská (200m)
- DM Drogéria, Obchodná (200m)
- Fresh Market, Laurinská 10

**Zdravotné zariadenia:**
- Všeobecná ambulancia, Laurinská 8
- Stomatológia Centrum, Laurinská 15
- Detský lekár, Ventúrska 3 (150m)

**MHD:**
- Zastávka Hodžovo námestie (200m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zochova (300m) - linky 93, 203, 207

**Parky a oddych:**
- Hviezdoslavovo námestie (350m, 5 min chôdze)
- Medická záhrada (500m, 7 min chôdze)
- Sad Janka Kráľa (800m)

**Reštaurácie a kaviarne:**
- Urban House, Laurinská 16
- Prasná Bašta, Zámočnícka 11 (200m)
- Leberfinger, Beblavého (250m)
- Altitude Restaurant (100m)

**Kultúrne zariadenia:**
- Slovenské národné divadlo (400m)
- Primaciálny palác (200m)
- Reduta (300m)`
    },
    'hviezdoslavovo námestie': {
      name: 'Hviezdoslavovo námestie',
      lat: 48.1420,
      lon: 17.1080,
      info: `Hviezdoslavovo námestie je reprezentatívne námestie s parkom v centre Bratislavy.

**Poloha a charakter:**
- Dlhé námestie s alejou stromov
- Park s fontánami a lavičkami
- Kultúrne centrum s divadlom

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (400m, 5 min chôdze)
- MŠ Grösslingova 23 (500m, 7 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (600m, 8 min chôdze)
- ZŠ Zochova 3 (400m, 5 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (400m, 5 min chôdze)
- Billa Obchodná (450m, 6 min chôdze)
- Lekáreň Salvator, Hlavné nám. (300m)
- DM Drogéria, Obchodná (400m)

**Zdravotné zariadenia:**
- Všeobecná ambulancia, Ventúrska 10 (250m)
- Poliklinika Grösslingova (500m)
- Detský lekár, Kapitulská 8 (350m)

**MHD:**
- Zastávka Zochova (200m) - linky 93, 203, 207
- Zastávka Hodžovo námestie (300m) - linky 201, 203, 205, 206

**Parky a oddych:**
- Park Hviezdoslavovo nám. (priamo na námestí)
- Sad Janka Kráľa (500m, 7 min chôdze)
- Medická záhrada (600m)

**Reštaurácie a kaviarne:**
- Café Verne, Hviezdoslavovo nám.
- Sky Bar & Restaurant, Hviezdoslavovo nám. 7
- Mezzo Mezzo, Rybárska brána
- Kolkovna, Hviezdoslavovo nám.

**Kultúrne zariadenia:**
- Slovenské národné divadlo (priamo na námestí)
- Reduta (50m)
- Kunsthalle (100m)`
    },
    'sedlárska': {
      name: 'Sedlárska',
      lat: 48.1448,
      lon: 17.1068,
      info: `Sedlárska ulica je historická ulica v centre Starého Mesta.

**Poloha a charakter:**
- Úzka historická ulica
- Typická stredoveká zástavba
- Tichšia lokalita v centre

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (300m, 4 min chôdze)
- MŠ Beblavého 1 (250m, 3 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (600m, 8 min chôdze)
- ZŠ Komenského nám. 6 (700m, 9 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (250m, 3 min chôdze)
- Billa Obchodná (400m, 5 min chôdze)
- Lekáreň Pod vežou, Michalská (150m)
- DM Drogéria, Obchodná (350m)

**Zdravotné zariadenia:**
- Všeobecná ambulancia, Ventúrska 10 (200m)
- Stomatológia, Sedlárska 5
- Detský lekár, Kapitulská 8 (250m)

**MHD:**
- Zastávka Hodžovo námestie (300m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zochova (250m) - linky 93, 203, 207

**Parky a oddych:**
- Hviezdoslavovo námestie (300m, 4 min chôdze)
- Prezidentská záhrada (600m)
- Medická záhrada (500m)

**Reštaurácie a kaviarne:**
- Bistro St. Germain, Sedlárska 6
- Vínna galéria, Sedlárska 8
- Pulitzer Café, Ventúrska (100m)

**Kultúrne zariadenia:**
- Primaciálny palác (200m)
- Slovenské národné divadlo (400m)
- Stará radnica (250m)`
    },
    'kapitulská': {
      name: 'Kapitulská',
      lat: 48.1425,
      lon: 17.1065,
      info: `Kapitulská ulica je tichá historická ulica v blízkosti Dómu sv. Martina.

**Poloha a charakter:**
- V blízkosti katedrály
- Pokojná obytná ulica
- Historické budovy a paláce

**Materské školy (do 500m):**
- MŠ Kapitulská 12 (priamo v ulici)
- MŠ Beblavého 1 (200m, 3 min chôdze)

**Základné školy (do 1km):**
- ZŠ Komenského nám. 6 (600m, 8 min chôdze)
- ZŠ Grösslingova 4 (700m, 9 min chôdze)

**Supermarkety a obchody:**
- Tesco Kamenné námestie (400m, 5 min chôdze)
- Fresh Market, Beblavého (250m)
- Lekáreň Salvator, Panenská (200m)
- Lekáreň Pod vežou, Michalská (300m)

**Zdravotné zariadenia:**
- Všeobecná ambulancia, Beblavého 10 (200m)
- Detský lekár MUDr. Nováková, Kapitulská 8 (priamo v ulici)
- Stomatológia, Panenská 5 (150m)

**MHD:**
- Zastávka Hodžovo námestie (350m) - linky 201, 203, 205, 206, 207, 208, 210
- Zastávka Zámocká (300m) - linky 203, 207, 208

**Parky a oddych:**
- Park pri Dóme sv. Martina (100m)
- Hradné sady (500m)
- Hviezdoslavovo námestie (400m)

**Reštaurácie a kaviarne:**
- Leberfinger, Beblavého 8 (200m)
- Hostinec U svätého Martina, Kapitulská
- Modra Hviezda, Beblavého 14 (150m)

**Kultúrne zariadenia:**
- Dóm sv. Martina (50m)
- Bratislavský hrad (700m)
- Dom U dobrého pastiera (400m)`
    },
    'hurbanovo námestie': {
      name: 'Hurbanovo námestie',
      lat: 48.1470,
      lon: 17.1095,
      info: `Hurbanovo námestie je dôležitý dopravný uzol na okraji Starého Mesta.

**Poloha a charakter:**
- Križovatka hlavných komunikácií
- Mix historických a moderných budov
- Výborná dostupnosť MHD

**Materské školy (do 500m):**
- MŠ Grösslingova 23 (300m, 4 min chôdze)
- MŠ Mudroňova 2 (400m, 5 min chôdze)

**Základné školy (do 1km):**
- ZŠ Grösslingova 4 (350m, 5 min chôdze)
- ZŠ Záborského 1 (600m, 8 min chôdze)

**Supermarkety a obchody:**
- Billa Obchodná (200m, 3 min chôdze)
- Tesco Kamenné námestie (300m, 4 min chôdze)
- DM Drogéria, Obchodná (200m)
- Lekáreň Dr. Max, Obchodná (150m)

**Zdravotné zariadenia:**
- Poliklinika Grösslingova (200m)
- Všeobecná ambulancia Dr. Varga, Grösslingova 8 (250m)
- Stomatológia, Obchodná 52 (150m)

**MHD:**
- Zastávka Hodžovo námestie (priamo na námestí) - linky 201, 203, 205, 206, 207, 208, 210
- Výborné prepojenie na celú Bratislavu

**Parky a oddych:**
- Medická záhrada (300m, 4 min chôdze)
- Hviezdoslavovo námestie (400m, 5 min chôdze)

**Reštaurácie a kaviarne:**
- Reštaurácie na Obchodnej (200m)
- Kaviarne v okolí
- Fast food reťazce (McDonald's, KFC)

**Kultúrne zariadenia:**
- Reduta (250m)
- Slovenské národné divadlo (400m)
- Galéria Nedbalka (200m)`
    }
  };

  const getStreetInfo = async (streetName) => {
    if (!streetName || !streetName.trim()) return null;
    
    const normalized = streetName.toLowerCase().trim();
    
    if (streetDatabase[normalized]) {
      console.log('Našiel som ulicu v databáze:', streetName);
      return {
        info: streetDatabase[normalized].info,
        lat: streetDatabase[normalized].lat,
        lon: streetDatabase[normalized].lon
      };
    }
    
    console.log('Ulica nie je v databáze:', streetName);
    return null;
  };

  const handleChange = (field, value) => {
    if (field === 'cena') {
      const cleaned = value.replace(/[^\d]/g, '');
      setFormData(prev => ({ ...prev, [field]: cleaned }));
    } else {
      setFormData(prev => ({ ...prev, [field]: value }));
    }
  };

  const analyzeData = async () => {
    if (!formData.cena || !formData.plocha) {
      setError('Prosím vyplňte aspoň Cena a Plocha');
      return;
    }

    setLoading(true);
    setError('');
    setShowMap(false);
    
    try {
      let streetInfo = null;
      
      if (formData.ulica && formData.ulica.trim()) {
        console.log('Vyhľadávam informácie o ulici:', formData.ulica);
        setShowMap(true);
        
        const streetData = await getStreetInfo(formData.ulica);
        if (streetData) {
          console.log('Získané informácie o ulici');
          streetInfo = streetData.info;
        } else {
          console.log('Informácie o ulici sa nepodarilo získať, ale mapa sa zobrazí');
        }
      }
      
      const result = generateAnalysis(streetInfo);
      setAnalysis(result);
    } catch (err) {
      console.error('Chyba:', err);
      setError('Nastala chyba pri analýze. Skúste to prosím znova.');
    } finally {
      setLoading(false);
    }
  };

  const generateAnalysis = (streetInfo) => {
    const isNewOrRenovated = formData.stav === 'Novostavba' || formData.stav === 'Po rekonštrukcii';
    
    const marketPricesNew = { "1": 5898, "2": 5891, "3": 6202, "4": 6000, "5": 5800, "5+": 5500 };
    const marketPricesOld = { "1": 4871, "2": 4798, "3": 4301, "4": 4200, "5": 4100, "5+": 4000 };
    
    const marketPrices = isNewOrRenovated ? marketPricesNew : marketPricesOld;
    const priceCategory = isNewOrRenovated ? "novostavieb a kompletne zrekonštruovaných bytov" : "pôvodných a čiastočne zrekonštruovaných bytov";
    
    const price = parseInt(formData.cena);
    const area = parseInt(formData.plocha);
    const pricePerM2 = Math.round(price / area);
    const marketPrice = marketPrices[formData.pocet_izieb] || null;
    
    const priceEval = `**Cena:** ${price.toLocaleString()} €
**Plocha:** ${area} m²
**Cena za m²:** ${pricePerM2.toLocaleString()} €/m²
**Trhová cena ${priceCategory}:** ${marketPrice ? marketPrice.toLocaleString() + ' €/m²' : 'neuvedené'}`;
    
    let priceComment = "";
    let barPosition = 50;
    let priceRating = 5;
    
    if (marketPrice) {
      const diff = pricePerM2 - marketPrice;
      const diffPercent = Math.round((diff / marketPrice) * 100);
      
      barPosition = Math.min(Math.max(((diffPercent + 30) / 60) * 100, 0), 100);
      
      if (diffPercent < -10) {
        priceComment = `**Výborná cena!** Nehnuteľnosť je ${Math.abs(diffPercent)}% pod trhovou cenou. Toto je veľmi zaujímavá ponuka.`;
        priceRating = 9;
      } else if (diffPercent < 0) {
        priceComment = `**Dobrá cena.** Nehnuteľnosť je ${Math.abs(diffPercent)}% pod trhovou cenou. Ponuka je v poriadku.`;
        priceRating = 7;
      } else if (diffPercent < 10) {
        priceComment = `**Trhová cena.** Nehnuteľnosť je na úrovni trhu, cena je ${diffPercent > 0 ? diffPercent + '% nad' : 'na úrovni'} trhu.`;
        priceRating = 6;
      } else if (diffPercent < 20) {
        priceComment = `**Mierne predražené.** Nehnuteľnosť je ${diffPercent}% nad trhovou cenou. Skúste vyjednávať nižšiu cenu.`;
        priceRating = 4;
      } else {
        priceComment = `**Výrazne predražené!** Nehnuteľnosť je ${diffPercent}% nad trhovou cenou. Neodporúčame bez výrazného zníženia ceny.`;
        priceRating = 2;
      }
    } else {
      priceComment = `Cena ${pricePerM2} €/m² pre ${priceCategory}. Odporúčame porovnať s podobnými bytmi v lokalite.`;
      priceRating = 5;
    }
    
    const priceBarometer = `__HTML_START__<div style="margin: 40px 0 20px 0;"><div style="position: relative; height: 40px; background: linear-gradient(to right, #22c55e 0%, #84cc16 20%, #eab308 40%, #f97316 60%, #ef4444 100%); border-radius: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);"><div style="position: absolute; left: ${barPosition}%; top: -35px; transform: translateX(-50%); text-align: center;"><div style="background: white; padding: 6px 14px; border-radius: 8px; font-weight: bold; color: #1e293b; box-shadow: 0 2px 8px rgba(0,0,0,0.2); white-space: nowrap; border: 2px solid #1e293b; margin-bottom: 2px;">${pricePerM2.toLocaleString()} €/m²</div><div style="width: 0; height: 0; border-left: 8px solid transparent; border-right: 8px solid transparent; border-top: 12px solid #1e293b; margin: 0 auto;"></div></div></div></div>__HTML_END__

${priceComment}`;

    let locationAnalysis = `**Staré Mesto** je srdcom Bratislavy a centrom spoločenského a kultúrneho diania hlavného mesta Slovenskej republiky.`;
    
    if (streetInfo) {
      locationAnalysis += `

**Informácie o ulici ${formData.ulica}:**

${streetInfo}

---
`;
    }
    
    locationAnalysis += `

**Základné údaje o Starom Meste:**
- Rozloha: 9,6 km² (tretia najmenšia, ale najhustejšie osídlená mestská časť)
- Počet obyvateľov: 46 929 (2022)
- Počet bytov: 28 248
- Hustota osídlenia: Najvyššia v Bratislave

**Výhody lokality:**
- **Historické centrum** - väčšina ikonických pamiatok (Bratislavský hrad, Dóm sv. Martina, Michalská veža, Modrý kostolík)
- **Pešia zóna** - Hlavné námestie, Hviezdoslavovo námestie, nábrežie Dunaja
- **Parky a oddych** - Medická záhrada, park Gábora Bárossa, Prezidentská záhrada
- **Reprezentatívnosť** - sídlo Národnej rady SR, vlády SR, prezidenta
- **Výborná dostupnosť** - centrum všetkého, pešo všade
- **Kultúra** - divadlá, galérie, múzeá, koncerty
- **Gastronómia** - množstvo reštaurácií, kaviarní, barov
- **MHD** - trolejbusy, autobusy, blízkosť Hlavnej stanice
- **Investičná hodnota** - najvyššia stabilita cien v Bratislave
- **Prestíž** - najvyhľadávanejšia adresa v hlavnom meste

**Nevýhody:**
- **Parkovanie** - veľmi obmedzené, vysoké poplatky za rezidentské parkovanie (cca 100-150 €/rok)
- **Hluk** - môže byť hlučnejšie, najmä v blízkosti barov a reštaurácií
- **Turisti** - vysoký turistický ruch, najmä v sezóne
- **Vyššie náklady** - drahšie služby, reštaurácie
- **Obmedzená zeleň** - menej zelených plôch v centre
- **Hustá zástavba** - vysoká hustota osídlenia

**Perspektíva:**
Staré Mesto je najnavštevovanejšou turistickou lokalitou na Slovensku a jeho hodnota neustále rastie. Je to najbezpečnejšia investícia do nehnuteľností v Bratislave s najstabilnejšími cenami.`;

    let missingData = [];
    if (!formData.pocet_izieb) missingData.push("počet izieb");
    if (!formData.vlastnictvo) missingData.push("typ vlastníctva");
    if (!formData.poschodie) missingData.push("poschodie");
    if (!formData.stav) missingData.push("stav nehnuteľnosti");
    if (!formData.popis || formData.popis.length < 50) missingData.push("detailný popis");

    const dataCompleteness = missingData.length === 0 
      ? "✅ Všetky základné informácie sú uvedené."
      : `⚠️ Chýbajúce informácie: ${missingData.join(", ")}. Tieto údaje by ste mali získať pred rozhodnutím.`;

    const redFlags = [];
    if (!formData.stav) redFlags.push("Nie je uvedený stav - môže skrývať potrebu rekonštrukcie");
    if (pricePerM2 < (marketPrice || 4000) * 0.7) redFlags.push("Podozrivo nízka cena - zistite dôvod");
    if (!formData.vlastnictvo) redFlags.push("Typ vlastníctva neznámy - dôležité pre hypotéku");
    if (!formData.poschodie) redFlags.push("Neznáme poschodie - ovplyvňuje cenu a komfort");
    if (!formData.popis || formData.popis.length < 50) redFlags.push("Chýba detailný popis - málo informácií");

    const redFlagsText = redFlags.length > 0 
      ? "🚩 " + redFlags.map(f => "• " + f).join("\n")
      : "✅ Žiadne zjavné červené vlajky pri základných údajoch.";

    const questions = `**Kľúčové otázky:**

1. Aké sú presné mesačné náklady? (energie, fond opráv, internet)
2. Kedy bola naposledy robená rekonštrukcia a čoho sa týkala?
3. Aký je stav okien? (plastové/drevené, kedy vymenené)
4. Je k bytu pivnica alebo komora?
5. Možnosti parkovania? Cena parkovacej karty pre Staré Mesto?
6. Hlučnosť z ulice? Izolované okná?
7. Stav spoločných priestorov domu?
8. Sú na byte nejaké dlhy alebo právne záťaže?
9. Prečo majiteľ predáva?
10. Sú na byte nejaké známe nedoplatky?`;

    const suitability = [];
    if (formData.pocet_izieb === "1" || formData.pocet_izieb === "2") {
      suitability.push("single, študenti, mladí profesionáli");
    }
    if (formData.pocet_izieb === "3" || formData.pocet_izieb === "4") {
      suitability.push("mladé rodiny, páry");
    }
    if (isNewOrRenovated) {
      suitability.push("ľudia hľadajúci moderné bývanie bez starostí s rekonštrukciou");
    }
    suitability.push("investori do prestížnej lokality");

    const recommendation = priceRating >= 7 
      ? "✅ Odporúčam - dobrá príležitosť, pokračujte ďalej."
      : priceRating >= 5 
      ? "⚠️ Zvážte - nie je to zlá ponuka, ale skúste vyjednávať."
      : "❌ Neodporúčam - hľadajte lepšie možnosti alebo výrazne vyjednávajte cenu.";

    const overallSection = `**Hodnotenie:** ${priceRating}/10

**Vhodné pre:** ${suitability.join(", ")}

**Odporúčanie:** ${recommendation}

${marketPrice ? '**Investičný potenciál:** Staré Mesto má stabilnú hodnotu a vysoký dopyt. Pri dobrej cene ide o solídnu investíciu.' : ''}`;

    return `## 1. Cena a hodnota

${priceEval}

${priceBarometer}

## 2. Lokalita

${locationAnalysis}

## 3. Kompletnosť údajov

${dataCompleteness}

${missingData.length > 0 ? 'Pre kompletnú analýzu potrebujeme viac informácií o nehnuteľnosti.' : ''}

## 4. Červené vlajky

${redFlagsText}

## 5. Otázky na prehliadku

${questions}

## 6. Celkové hodnotenie

${overallSection}`;
  };

  const resetForm = () => {
    setFormData({
      cena: '',
      pocet_izieb: '',
      lokalita: 'Bratislava - Stare mesto',
      ulica: '',
      plocha: '',
      vlastnictvo: '',
      poschodie: '',
      stav: '',
      popis: ''
    });
    setAnalysis('');
    setError('');
    setShowMap(false);
  };

  const fillExample = () => {
    setFormData({
      cena: '185000',
      pocet_izieb: '3',
      lokalita: 'Bratislava - Stare mesto',
      ulica: 'Panská',
      plocha: '68',
      vlastnictvo: 'Osobné',
      poschodie: '4/8',
      stav: 'Pôvodný',
      popis: 'Priestranný 3-izbový byt v zateplenom panelovom dome s výťahom. Dispozícia: vstupná chodba, kúpeľňa s WC, tri izby, lodžia. Výborná občianska vybavenosť, obchody, škôlky, školy v blízkosti. Zastávka MHD 2 min chôdze.'
    });
    setError('');
  };

  const formatAnalysis = (text) => {
    const lines = text.split('\n');
    const result = [];
    let bulletPoints = [];
    
    for (let idx = 0; idx < lines.length; idx++) {
      const line = lines[idx];
      
      if (line.trim() === '---') {
        if (bulletPoints.length > 0) {
          result.push(<ul key={`ul-${idx}`} className="list-disc ml-6 mb-4">{bulletPoints}</ul>);
          bulletPoints = [];
        }
        result.push(<hr key={idx} className="my-6 border-gray-300" />);
        continue;
      }
      
      if (line.includes('__HTML_START__')) {
        if (bulletPoints.length > 0) {
          result.push(<ul key={`ul-${idx}`} className="list-disc ml-6 mb-4">{bulletPoints}</ul>);
          bulletPoints = [];
        }
        const htmlContent = line.replace('__HTML_START__', '').replace('__HTML_END__', '');
        result.push(<div key={idx} dangerouslySetInnerHTML={{__html: htmlContent}} />);
        continue;
      }
      
      if (line.trim().startsWith('- ')) {
        const content = line.trim().substring(2);
        const parts = content.split(/(\*\*.*?\*\*)/g);
        bulletPoints.push(
          <li key={`li-${idx}`} className="mb-2 leading-relaxed text-gray-700">
            {parts.map((part, i) => {
              if (part.startsWith('**') && part.endsWith('**')) {
                return <strong key={i} className="font-semibold text-gray-900">{part.slice(2, -2)}</strong>;
              }
              return <span key={i}>{part}</span>;
            })}
          </li>
        );
        continue;
      }
      
      if (bulletPoints.length > 0) {
        result.push(<ul key={`ul-${idx}`} className="list-disc ml-6 mb-4">{bulletPoints}</ul>);
        bulletPoints = [];
      }
      
      if (line.trim().startsWith('## ')) {
        result.push(
          <h3 key={idx} className="text-xl font-bold mt-8 mb-4 text-indigo-700 border-b-2 border-indigo-200 pb-2">
            {line.replace(/^##\s*/, '')}
          </h3>
        );
        continue;
      }
      
      if (line.trim().startsWith('**') && line.trim().endsWith('**')) {
        result.push(
          <h3 key={idx} className="text-lg font-bold mt-6 mb-3 text-gray-800">
            {line.replace(/\*\*/g, '')}
          </h3>
        );
        continue;
      }
      
      if (line.trim()) {
        const parts = line.split(/(\*\*.*?\*\*)/g);
        result.push(
          <p key={idx} className="mb-4 leading-relaxed text-gray-700">
            {parts.map((part, i) => {
              if (part.startsWith('**') && part.endsWith('**')) {
                return <strong key={i} className="font-semibold text-gray-900">{part.slice(2, -2)}</strong>;
              }
              return <span key={i}>{part}</span>;
            })}
          </p>
        );
        continue;
      }
      
      result.push(<div key={idx} className="h-2" />);
    }
    
    if (bulletPoints.length > 0) {
      result.push(<ul key={`ul-final`} className="list-disc ml-6 mb-4">{bulletPoints}</ul>);
    }
    
    return result;
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 via-indigo-50 to-purple-50 p-4 relative overflow-hidden">
      <div className="absolute inset-0 overflow-hidden pointer-events-none">
        <div className="absolute w-96 h-96 bg-indigo-200 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-blob top-0 -left-4"></div>
        <div className="absolute w-96 h-96 bg-purple-200 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-blob animation-delay-2000 top-0 -right-4"></div>
        <div className="absolute w-96 h-96 bg-pink-200 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-blob animation-delay-4000 bottom-0 left-20"></div>
      </div>
      
      <style>{`
        @keyframes blob {
          0% { transform: translate(0px, 0px) scale(1); }
          33% { transform: translate(30px, -50px) scale(1.1); }
          66% { transform: translate(-20px, 20px) scale(0.9); }
          100% { transform: translate(0px, 0px) scale(1); }
        }
        .animate-blob { animation: blob 7s infinite; }
        .animation-delay-2000 { animation-delay: 2s; }
        .animation-delay-4000 { animation-delay: 4s; }
      `}</style>

      <div className="max-w-4xl mx-auto relative z-10">
        <div className="text-center mb-8 pt-8">
          <div className="flex items-center justify-center gap-4 mb-4">
            <div className="w-28 h-auto">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 375 374.999991" className="w-full h-auto">
                <g transform="matrix(1, 0, 0, 1, 12, 6)">
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(1.014903, 167.805393)">
                      <g>
                        <path d="M 63.296875 4.789062 C 52.285156 4.789062 42.199219 2.058594 33.046875 -3.398438 C 23.898438 -8.851562 16.691406 -16.378906 11.421875 -25.980469 C 6.167969 -35.554688 3.539062 -46.328125 3.539062 -58.296875 C 3.539062 -70.269531 6.167969 -81.042969 11.421875 -90.617188 C 16.691406 -100.21875 23.898438 -107.746094 33.046875 -113.199219 C 42.199219 -118.65625 52.285156 -121.386719 63.296875 -121.386719 C 74.308594 -121.386719 84.390625 -118.65625 93.542969 -113.199219 C 102.691406 -107.746094 109.902344 -100.21875 115.171875 -90.617188 C 120.425781 -81.042969 123.050781 -70.269531 123.050781 -58.296875 C 123.050781 -46.328125 120.425781 -35.554688 115.171875 -25.980469 C 109.902344 -16.378906 102.691406 -8.851562 93.542969 -3.398438 C 84.390625 2.058594 74.308594 4.789062 63.296875 4.789062 Z M 63.296875 -1.457031 C 73.160156 -1.457031 82.175781 -3.890625 90.34375 -8.761719 C 98.519531 -13.636719 104.96875 -20.378906 109.695312 -28.984375 C 114.433594 -37.625 116.804688 -47.394531 116.804688 -58.296875 C 116.804688 -69.203125 114.433594 -78.972656 109.695312 -87.609375 C 104.96875 -96.21875 98.519531 -102.960938 90.34375 -107.835938 C 82.175781 -112.703125 73.160156 -115.140625 63.296875 -115.140625 C 53.429688 -115.140625 44.414062 -112.703125 36.246094 -107.835938 C 28.070312 -102.960938 21.621094 -96.21875 16.894531 -87.609375 C 12.15625 -78.972656 9.785156 -69.203125 9.785156 -58.296875 C 9.785156 -47.394531 12.15625 -37.625 16.894531 -28.984375 C 21.621094 -20.378906 28.070312 -13.636719 36.246094 -8.761719 C 44.414062 -3.890625 53.429688 -1.457031 63.296875 -1.457031 Z M 63.296875 -27.273438 C 68.253906 -27.273438 72.753906 -28.535156 76.804688 -31.050781 C 80.867188 -33.582031 84.101562 -37.171875 86.503906 -41.824219 C 88.9375 -46.539062 90.152344 -52.03125 90.152344 -58.296875 C 90.152344 -64.566406 88.9375 -70.058594 86.503906 -74.773438 C 84.101562 -79.425781 80.867188 -83.015625 76.804688 -85.542969 C 72.753906 -88.0625 68.253906 -89.320312 63.296875 -89.320312 C 58.335938 -89.320312 53.835938 -88.0625 49.789062 -85.542969 C 45.722656 -83.015625 42.488281 -79.425781 40.085938 -74.773438 C 37.652344 -70.058594 36.4375 -64.566406 36.4375 -58.296875 C 36.4375 -52.03125 37.652344 -46.539062 40.085938 -41.824219 C 42.488281 -37.171875 45.722656 -33.582031 49.789062 -31.050781 C 53.835938 -28.535156 58.335938 -27.273438 63.296875 -27.273438 Z M 63.296875 -21.027344 C 57.148438 -21.027344 51.546875 -22.601562 46.488281 -25.746094 C 41.449219 -28.882812 37.464844 -33.285156 34.535156 -38.960938 C 31.636719 -44.574219 30.191406 -51.019531 30.191406 -58.296875 C 30.191406 -65.578125 31.636719 -72.023438 34.535156 -77.636719 C 37.464844 -83.308594 41.449219 -87.714844 46.488281 -90.847656 C 51.546875 -93.996094 57.148438 -95.566406 63.296875 -95.566406 C 69.441406 -95.566406 75.042969 -93.996094 80.101562 -90.847656 C 85.140625 -87.714844 89.125 -83.308594 92.054688 -77.636719 C 94.953125 -72.023438 96.402344 -65.578125 96.402344 -58.296875 C 96.402344 -51.019531 94.953125 -44.574219 92.054688 -38.960938 C 89.125 -33.285156 85.140625 -28.882812 80.101562 -25.746094 C 75.042969 -22.601562 69.441406 -21.027344 63.296875 -21.027344 Z M 63.296875 -21.027344 "/>
                      </g>
                    </g>
                  </g>
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(127.602807, 167.805393)">
                      <g>
                        <path d="M 38.308594 3.125 L 8.535156 3.125 L 8.535156 -119.71875 L 46.03125 -119.71875 L 83.550781 -48.085938 L 80.785156 -46.636719 L 80.785156 -49.761719 L 84.117188 -49.761719 L 84.117188 -46.636719 L 80.992188 -46.636719 L 80.992188 -119.71875 L 113.890625 -119.71875 L 113.890625 3.125 L 76.398438 3.125 L 38.875 -68.507812 L 41.640625 -69.957031 L 41.640625 -66.835938 L 38.308594 -66.835938 L 38.308594 -69.957031 L 41.433594 -69.957031 L 41.433594 3.125 Z M 38.308594 -3.125 L 38.308594 0 L 35.1875 0 L 35.1875 -73.082031 L 43.53125 -73.082031 L 81.054688 -1.449219 L 78.285156 0 L 78.285156 -3.125 L 110.765625 -3.125 L 110.765625 0 L 107.644531 0 L 107.644531 -116.597656 L 110.765625 -116.597656 L 110.765625 -113.472656 L 84.117188 -113.472656 L 84.117188 -116.597656 L 87.238281 -116.597656 L 87.238281 -43.515625 L 78.894531 -43.515625 L 41.375 -115.148438 L 44.140625 -116.597656 L 44.140625 -113.472656 L 11.660156 -113.472656 L 11.660156 -116.597656 L 14.78125 -116.597656 L 14.78125 0 L 11.660156 0 L 11.660156 -3.125 Z M 38.308594 -3.125 "/>
                      </g>
                    </g>
                  </g>
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(250.026621, 167.805393)">
                      <g>
                        <path d="M 91.613281 3.125 L 8.535156 3.125 L 8.535156 -119.71875 L 94.734375 -119.71875 L 94.734375 -89.320312 L 38.308594 -89.320312 L 38.308594 -92.445312 L 41.433594 -92.445312 L 41.433594 -69.957031 L 38.308594 -69.957031 L 38.308594 -73.082031 L 91.402344 -73.082031 L 91.402344 -43.515625 L 38.308594 -43.515625 L 38.308594 -46.636719 L 41.433594 -46.636719 L 41.433594 -24.152344 L 38.308594 -24.152344 L 38.308594 -27.273438 L 94.734375 -27.273438 L 94.734375 3.125 Z M 91.613281 -3.125 L 91.613281 0 L 88.488281 0 L 88.488281 -24.152344 L 91.613281 -24.152344 L 91.613281 -21.027344 L 35.1875 -21.027344 L 35.1875 -49.761719 L 88.28125 -49.761719 L 88.28125 -46.636719 L 85.15625 -46.636719 L 85.15625 -69.957031 L 88.28125 -69.957031 L 88.28125 -66.835938 L 35.1875 -66.835938 L 35.1875 -95.566406 L 91.613281 -95.566406 L 91.613281 -92.445312 L 88.488281 -92.445312 L 88.488281 -116.597656 L 91.613281 -116.597656 L 91.613281 -113.472656 L 11.660156 -113.472656 L 11.660156 -116.597656 L 14.78125 -116.597656 L 14.78125 0 L 11.660156 0 L 11.660156 -3.125 Z M 91.613281 -3.125 "/>
                      </g>
                    </g>
                  </g>
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(5.181181, 317.64036)">
                      <g>
                        <path d="M 90.777344 3.125 L 8.535156 3.125 L 8.535156 -119.71875 L 41.433594 -119.71875 L 41.433594 -25.816406 L 38.308594 -25.816406 L 38.308594 -28.941406 L 93.902344 -28.941406 L 93.902344 3.125 Z M 90.777344 -3.125 L 90.777344 0 L 87.65625 0 L 87.65625 -25.816406 L 90.777344 -25.816406 L 90.777344 -22.695312 L 35.1875 -22.695312 L 35.1875 -116.597656 L 38.308594 -116.597656 L 38.308594 -113.472656 L 11.660156 -113.472656 L 11.660156 -116.597656 L 14.78125 -116.597656 L 14.78125 0 L 11.660156 0 L 11.660156 -3.125 Z M 90.777344 -3.125 "/>
                      </g>
                    </g>
                  </g>
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(98.456488, 317.64036)">
                      <g>
                        <path d="M 8.535156 -119.71875 L 41.433594 -119.71875 L 41.433594 3.125 L 8.535156 3.125 Z M 14.78125 -113.472656 L 14.78125 -3.125 L 35.1875 -3.125 L 35.1875 -113.472656 Z M 14.78125 -113.472656 "/>
                      </g>
                    </g>
                  </g>
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(148.425397, 317.64036)">
                      <g>
                        <path d="M 38.308594 3.125 L 8.535156 3.125 L 8.535156 -119.71875 L 93.902344 -119.71875 L 93.902344 -89.320312 L 38.308594 -89.320312 L 38.308594 -92.445312 L 41.433594 -92.445312 L 41.433594 -69.125 L 38.308594 -69.125 L 38.308594 -72.25 L 89.738281 -72.25 L 89.738281 -42.683594 L 38.308594 -42.683594 L 38.308594 -45.804688 L 41.433594 -45.804688 L 41.433594 3.125 Z M 38.308594 -3.125 L 38.308594 0 L 35.1875 0 L 35.1875 -48.929688 L 86.613281 -48.929688 L 86.613281 -45.804688 L 83.492188 -45.804688 L 83.492188 -69.125 L 86.613281 -69.125 L 86.613281 -66 L 35.1875 -66 L 35.1875 -95.566406 L 90.777344 -95.566406 L 90.777344 -92.445312 L 87.65625 -92.445312 L 87.65625 -116.597656 L 90.777344 -116.597656 L 90.777344 -113.472656 L 11.660156 -113.472656 L 11.660156 -116.597656 L 14.78125 -116.597656 L 14.78125 0 L 11.660156 0 L 11.660156 -3.125 Z M 38.308594 -3.125 "/>
                      </g>
                    </g>
                  </g>
                  <g fill="#222e30" fillOpacity="1">
                    <g transform="translate(245.864769, 317.64036)">
                      <g>
                        <path d="M 91.613281 3.125 L 8.535156 3.125 L 8.535156 -119.71875 L 94.734375 -119.71875 L 94.734375 -89.320312 L 38.308594 -89.320312 L 38.308594 -92.445312 L 41.433594 -92.445312 L 41.433594 -69.957031 L 38.308594 -69.957031 L 38.308594 -73.082031 L 91.402344 -73.082031 L 91.402344 -43.515625 L 38.308594 -43.515625 L 38.308594 -46.636719 L 41.433594 -46.636719 L 41.433594 -24.152344 L 38.308594 -24.152344 L 38.308594 -27.273438 L 94.734375 -27.273438 L 94.734375 3.125 Z M 91.613281 -3.125 L 91.613281 0 L 88.488281 0 L 88.488281 -24.152344 L 91.613281 -24.152344 L 91.613281 -21.027344 L 35.1875 -21.027344 L 35.1875 -49.761719 L 88.28125 -49.761719 L 88.28125 -46.636719 L 85.15625 -46.636719 L 85.15625 -69.957031 L 88.28125 -69.957031 L 88.28125 -66.835938 L 35.1875 -66.835938 L 35.1875 -95.566406 L 91.613281 -95.566406 L 91.613281 -92.445312 L 88.488281 -92.445312 L 88.488281 -116.597656 L 91.613281 -116.597656 L 91.613281 -113.472656 L 11.660156 -113.472656 L 11.660156 -116.597656 L 14.78125 -116.597656 L 14.78125 0 L 11.660156 0 L 11.660156 -3.125 Z M 91.613281 -3.125 "/>
                      </g>
                    </g>
                  </g>
                </g>
              </svg>
            </div>
          </div>
          <div className="inline-block bg-gray-900 text-white px-6 py-2 rounded-full mb-4 shadow-lg">
            <span className="text-sm font-semibold tracking-wide">www.onelifelimited.com</span>
          </div>
          <h1 className="text-3xl md:text-4xl font-bold text-gray-800 mb-2">
            ANALYZÁTOR NEHNUTEĽNOSTÍ
          </h1>
          <p className="text-xl text-gray-700 font-semibold">
            Bratislava - Staré mesto
          </p>
          <p className="text-gray-600 text-base mt-2">
            Vyplňte údaje o nehnuteľnosti a získajte okamžitú AI analýzu
          </p>
        </div>

        {!analysis && (
          <div className="bg-white rounded-2xl shadow-xl p-6 md:p-8 mb-6">
            <div className="flex items-center justify-between mb-6">
              <h2 className="text-2xl font-bold text-gray-800">Údaje o nehnuteľnosti</h2>
              <button
                onClick={fillExample}
                className="text-sm text-indigo-600 hover:text-indigo-700 font-semibold px-4 py-2 rounded-lg hover:bg-indigo-50"
              >
                Vyplniť ukážku
              </button>
            </div>

            <div className="grid md:grid-cols-2 gap-6">
              <div>
                <label className="block text-gray-700 font-semibold mb-2">
                  Cena <span className="text-red-500">*</span>
                </label>
                <div className="relative">
                  <input
                    type="text"
                    value={formData.cena}
                    onChange={(e) => handleChange('cena', e.target.value)}
                    placeholder="185000"
                    className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
                  />
                  <span className="absolute right-4 top-3 text-gray-500 font-semibold">€</span>
                </div>
                <p className="text-xs text-gray-500 mt-1">Zadajte len čísla bez bodiek a čiarok</p>
              </div>

              <div>
                <label className="block text-gray-700 font-semibold mb-2">
                  Počet izieb
                </label>
                <select
                  value={formData.pocet_izieb}
                  onChange={(e) => handleChange('pocet_izieb', e.target.value)}
                  className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none appearance-none bg-white"
                  style={{backgroundImage: `url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%23666' d='M6 9L1 4h10z'/%3E%3C/svg%3E")`, backgroundRepeat: 'no-repeat', backgroundPosition: 'right 1rem center'}}
                >
                  <option value="">Vyberte...</option>
                  <option value="1">1-izbový</option>
                  <option value="2">2-izbový</option>
                  <option value="3">3-izbový</option>
                  <option value="4">4-izbový</option>
                  <option value="5">5-izbový</option>
                  <option value="5+">5+ izbový</option>
                </select>
              </div>

              <div>
                <label className="block text-gray-700 font-semibold mb-2">
                  Ulica
                </label>
                <input
                  type="text"
                  value={formData.ulica}
                  onChange={(e) => handleChange('ulica', e.target.value)}
                  placeholder="napr. Panská, Michalská..."
                  className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
                />
                <p className="text-xs text-gray-500 mt-1">Získame informácie o okolí</p>
              </div>

              <div>
                <label className="block text-gray-700 font-semibold mb-2">
                  Plocha <span className="text-red-500">*</span>
                </label>
                <div className="relative">
                  <input
                    type="text"
                    value={formData.plocha}
                    onChange={(e) => handleChange('plocha', e.target.value)}
                    placeholder="68"
                    className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
                  />
                  <span className="absolute right-4 top-3 text-gray-500 font-semibold">m²</span>
                </div>
              </div>

              <div>
                <label className="block text-gray-700 font-semibold mb-2">
                  Vlastníctvo
                </label>
                <select
                  value={formData.vlastnictvo}
                  onChange={(e) => handleChange('vlastnictvo', e.target.value)}
                  className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none appearance-none bg-white"
                  style={{backgroundImage: `url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%23666' d='M6 9L1 4h10z'/%3E%3C/svg%3E")`, backgroundRepeat: 'no-repeat', backgroundPosition: 'right 1rem center'}}
                >
                  <option value="">Vyberte...</option>
                  <option value="Osobné">Osobné</option>
                  <option value="Družstevné">Družstevné</option>
                  <option value="Iné">Iné</option>
                </select>
              </div>

              <div>
                <label className="block text-gray-700 font-semibold mb-2">
                  Poschodie
                </label>
                <input
                  type="text"
                  value={formData.poschodie}
                  onChange={(e) => handleChange('poschodie', e.target.value)}
                  placeholder="4/8"
                  className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none"
                />
              </div>

              <div className="md:col-span-2">
                <label className="block text-gray-700 font-semibold mb-2">
                  Stav
                </label>
                <select
                  value={formData.stav}
                  onChange={(e) => handleChange('stav', e.target.value)}
                  className="w-full h-12 px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none appearance-none bg-white"
                  style={{backgroundImage: `url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%23666' d='M6 9L1 4h10z'/%3E%3C/svg%3E")`, backgroundRepeat: 'no-repeat', backgroundPosition: 'right 1rem center'}}
                >
                  <option value="">Vyberte...</option>
                  <option value="Novostavba">Novostavba</option>
                  <option value="Po rekonštrukcii">Po rekonštrukcii</option>
                  <option value="Pôvodný">Pôvodný</option>
                  <option value="Čiastočná rekonštrukcia">Čiastočná rekonštrukcia</option>
                </select>
              </div>

              <div className="md:col-span-2">
                <label className="block text-gray-700 font-semibold mb-2">
                  Popis
                </label>
                <textarea
                  value={formData.popis}
                  onChange={(e) => handleChange('popis', e.target.value)}
                  placeholder="Popis nehnuteľnosti, dispozícia, vybavenie, výhody lokality..."
                  rows="5"
                  className="w-full px-4 py-3 border-2 border-gray-300 rounded-lg focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200 focus:outline-none resize-none"
                />
              </div>
            </div>

            {error && (
              <div className="mt-6 p-4 bg-red-50 border-l-4 border-red-500 rounded-lg flex items-start gap-3 text-red-700">
                <AlertCircle className="w-5 h-5 flex-shrink-0 mt-0.5" />
                <span>{error}</span>
              </div>
            )}

            <button
              onClick={analyzeData}
              disabled={loading || !formData.cena || !formData.plocha}
              className="mt-6 w-full bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 disabled:from-gray-400 disabled:to-gray-400 text-white font-bold py-4 px-6 rounded-xl transition-all duration-200 flex items-center justify-center gap-2 shadow-lg hover:shadow-xl transform hover:-translate-y-0.5 disabled:transform-none"
            >
              {loading ? (
                <>
                  <div className="w-5 h-5 border-3 border-white border-t-transparent rounded-full animate-spin" />
                  Analyzujem...
                </>
              ) : (
                <>
                  <TrendingUp className="w-5 h-5" />
                  Analyzovať nehnuteľnosť
                </>
              )}
            </button>
          </div>
        )}

        {analysis && (
          <div className="bg-white rounded-2xl shadow-xl p-6 md:p-8 mb-8">
            <div className="flex items-center gap-3 mb-6 pb-4 border-b-2 border-indigo-100">
              <CheckCircle className="w-7 h-7 text-green-600" />
              <h2 className="text-3xl font-bold text-gray-800">
                Odborná AI analýza
              </h2>
            </div>

            <div className="mb-6 p-4 bg-gray-50 rounded-lg">
              <h3 className="font-semibold text-gray-700 mb-3">Analyzované údaje:</h3>
              <div className="grid grid-cols-2 md:grid-cols-4 gap-3 text-sm">
                {formData.cena && <div><span className="text-gray-600">Cena:</span> <strong>{parseInt(formData.cena).toLocaleString()} €</strong></div>}
                {formData.pocet_izieb && <div><span className="text-gray-600">Izby:</span> <strong>{formData.pocet_izieb}</strong></div>}
                {formData.plocha && <div><span className="text-gray-600">Plocha:</span> <strong>{formData.plocha} m²</strong></div>}
                {formData.vlastnictvo && <div><span className="text-gray-600">Vlastníctvo:</span> <strong>{formData.vlastnictvo}</strong></div>}
              </div>
              {formData.lokalita && <div className="mt-2 text-sm"><span className="text-gray-600">Lokalita:</span> <strong>{formData.lokalita}</strong></div>}
              {formData.ulica && <div className="mt-1 text-sm"><span className="text-gray-600">Ulica:</span> <strong>{formData.ulica}</strong></div>}
            </div>

            {showMap && formData.ulica && (
              <div className="mb-6 p-4 bg-indigo-50 rounded-lg border-2 border-indigo-200">
                <h3 className="font-semibold text-gray-800 mb-3 flex items-center gap-2">
                  <MapPin className="w-5 h-5 text-indigo-600" />
                  Poloha nehnuteľnosti
                </h3>
                <p className="text-sm text-gray-700 mb-4">
                  📍 <strong>{formData.ulica}, Bratislava - Staré Mesto</strong>
                </p>
                <div className="bg-white p-4 rounded-lg">
                  <a 
                    href={`https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(formData.ulica + ', Bratislava - Staré Mesto, Slovakia')}`}
                    target="_blank"
                    rel="noopener noreferrer"
                    className="inline-flex items-center gap-2 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-3 px-6 rounded-lg transition-colors shadow-md hover:shadow-lg"
                  >
                    <MapPin className="w-5 h-5" />
                    Zobraziť na mape Google Maps
                  </a>
                  <p className="text-xs text-gray-500 mt-3">
                    Kliknutím sa otvorí interaktívna mapa s označením ulice a okolia
                  </p>
                </div>
              </div>
            )}

            <div className="prose prose-lg max-w-none">
              {formatAnalysis(analysis)}
            </div>
            <button
              onClick={resetForm}
              className="mt-6 w-full bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-3 px-6 rounded-xl transition-colors"
            >
              Analyzovať ďalšiu nehnuteľnosť
            </button>
          </div>
        )}

        {!analysis && !loading && (
          <div className="grid md:grid-cols-3 gap-6 mb-8">
            <div className="bg-white rounded-xl shadow-lg p-6">
              <DollarSign className="w-10 h-10 text-indigo-600 mb-4" />
              <h3 className="font-bold text-gray-800 mb-2 text-lg">Analýza Ceny</h3>
              <p className="text-sm text-gray-600">
                Posúdenie primeranosti ceny
              </p>
            </div>
            <div className="bg-white rounded-xl shadow-lg p-6">
              <MapPin className="w-10 h-10 text-indigo-600 mb-4" />
              <h3 className="font-bold text-gray-800 mb-2 text-lg">Lokalita</h3>
              <p className="text-sm text-gray-600">
                Výhody a nevýhody
              </p>
            </div>
            <div className="bg-white rounded-xl shadow-lg p-6">
              <AlertCircle className="w-10 h-10 text-indigo-600 mb-4" />
              <h3 className="font-bold text-gray-800 mb-2 text-lg">Odporúčania</h3>
              <p className="text-sm text-gray-600">
                Otázky na prehliadku
              </p>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}