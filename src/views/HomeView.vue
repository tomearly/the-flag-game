<script setup>
import { computed, ref } from 'vue';
import { Position, getBearing, getDistance } from "aviation-math";

function getCardinal(angle) {
  /** 
   * Customize by changing the number of directions you have
   * We have 8
   */
  const degreePerDirection = 360 / 8;

  /** 
   * Offset the angle by half of the degrees per direction
   * Example: in 4 direction system North (320-45) becomes (0-90)
   */
  const offsetAngle = angle + degreePerDirection / 2;

  return (offsetAngle >= 0 * degreePerDirection && offsetAngle < 1 * degreePerDirection) ? "North"
    : (offsetAngle >= 1 * degreePerDirection && offsetAngle < 2 * degreePerDirection) ? "North East"
      : (offsetAngle >= 2 * degreePerDirection && offsetAngle < 3 * degreePerDirection) ? "East"
        : (offsetAngle >= 3 * degreePerDirection && offsetAngle < 4 * degreePerDirection) ? "South East"
          : (offsetAngle >= 4 * degreePerDirection && offsetAngle < 5 * degreePerDirection) ? "South "
            : (offsetAngle >= 5 * degreePerDirection && offsetAngle < 6 * degreePerDirection) ? "South West"
              : (offsetAngle >= 6 * degreePerDirection && offsetAngle < 7 * degreePerDirection) ? "West"
                : "NW";
}


const country_codes = [{"Name":"Albania","Code":"AL","alpha3":"ALB","numeric":8,"latitude":41,"longitude":20},{"Name":"Algeria","Code":"DZ","alpha3":"DZA","numeric":12,"latitude":28,"longitude":3},{"Name":"American Samoa","Code":"AS","alpha3":"ASM","numeric":16,"latitude":-14.3333,"longitude":-170},{"Name":"Andorra","Code":"AD","alpha3":"AND","numeric":20,"latitude":42.5,"longitude":1.6},{"Name":"Angola","Code":"AO","alpha3":"AGO","numeric":24,"latitude":-12.5,"longitude":18.5},{"Name":"Anguilla","Code":"AI","alpha3":"AIA","numeric":660,"latitude":18.25,"longitude":-63.1667},{"Name":"Antarctica","Code":"AQ","alpha3":"ATA","numeric":10,"latitude":-90,"longitude":0},{"Name":"Antigua and Barbuda","Code":"AG","alpha3":"ATG","numeric":28,"latitude":17.05,"longitude":-61.8},{"Name":"Argentina","Code":"AR","alpha3":"ARG","numeric":32,"latitude":-34,"longitude":-64},{"Name":"Armenia","Code":"AM","alpha3":"ARM","numeric":51,"latitude":40,"longitude":45},{"Name":"Aruba","Code":"AW","alpha3":"ABW","numeric":533,"latitude":12.5,"longitude":-69.9667},{"Name":"Australia","Code":"AU","alpha3":"AUS","numeric":36,"latitude":-27,"longitude":133},{"Name":"Austria","Code":"AT","alpha3":"AUT","numeric":40,"latitude":47.3333,"longitude":13.3333},{"Name":"Azerbaijan","Code":"AZ","alpha3":"AZE","numeric":31,"latitude":40.5,"longitude":47.5},{"Name":"Bahamas","Code":"BS","alpha3":"BHS","numeric":44,"latitude":24.25,"longitude":-76},{"Name":"Bahrain","Code":"BH","alpha3":"BHR","numeric":48,"latitude":26,"longitude":50.55},{"Name":"Bangladesh","Code":"BD","alpha3":"BGD","numeric":50,"latitude":24,"longitude":90},{"Name":"Barbados","Code":"BB","alpha3":"BRB","numeric":52,"latitude":13.1667,"longitude":-59.5333},{"Name":"Belarus","Code":"BY","alpha3":"BLR","numeric":112,"latitude":53,"longitude":28},{"Name":"Belgium","Code":"BE","alpha3":"BEL","numeric":56,"latitude":50.8333,"longitude":4},{"Name":"Belize","Code":"BZ","alpha3":"BLZ","numeric":84,"latitude":17.25,"longitude":-88.75},{"Name":"Benin","Code":"BJ","alpha3":"BEN","numeric":204,"latitude":9.5,"longitude":2.25},{"Name":"Bermuda","Code":"BM","alpha3":"BMU","numeric":60,"latitude":32.3333,"longitude":-64.75},{"Name":"Bhutan","Code":"BT","alpha3":"BTN","numeric":64,"latitude":27.5,"longitude":90.5},{"Name":"Bolivia, Plurinational State of","Code":"BO","alpha3":"BOL","numeric":68,"latitude":-17,"longitude":-65},{"Name":"Bosnia and Herzegovina","Code":"BA","alpha3":"BIH","numeric":70,"latitude":44,"longitude":18},{"Name":"Botswana","Code":"BW","alpha3":"BWA","numeric":72,"latitude":-22,"longitude":24},{"Name":"Bouvet Island","Code":"BV","alpha3":"BVT","numeric":74,"latitude":-54.4333,"longitude":3.4},{"Name":"Brazil","Code":"BR","alpha3":"BRA","numeric":76,"latitude":-10,"longitude":-55},{"Name":"British Indian Ocean Territory","Code":"IO","alpha3":"IOT","numeric":86,"latitude":-6,"longitude":71.5},{"Name":"Brunei Darussalam","Code":"BN","alpha3":"BRN","numeric":96,"latitude":4.5,"longitude":114.6667},{"Name":"Bulgaria","Code":"BG","alpha3":"BGR","numeric":100,"latitude":43,"longitude":25},{"Name":"Burkina Faso","Code":"BF","alpha3":"BFA","numeric":854,"latitude":13,"longitude":-2},{"Name":"Burundi","Code":"BI","alpha3":"BDI","numeric":108,"latitude":-3.5,"longitude":30},{"Name":"Cambodia","Code":"KH","alpha3":"KHM","numeric":116,"latitude":13,"longitude":105},{"Name":"Cameroon","Code":"CM","alpha3":"CMR","numeric":120,"latitude":6,"longitude":12},{"Name":"Canada","Code":"CA","alpha3":"CAN","numeric":124,"latitude":60,"longitude":-95},{"Name":"Cape Verde","Code":"CV","alpha3":"CPV","numeric":132,"latitude":16,"longitude":-24},{"Name":"Cayman Islands","Code":"KY","alpha3":"CYM","numeric":136,"latitude":19.5,"longitude":-80.5},{"Name":"Central African Republic","Code":"CF","alpha3":"CAF","numeric":140,"latitude":7,"longitude":21},{"Name":"Chad","Code":"TD","alpha3":"TCD","numeric":148,"latitude":15,"longitude":19},{"Name":"Chile","Code":"CL","alpha3":"CHL","numeric":152,"latitude":-30,"longitude":-71},{"Name":"China","Code":"CN","alpha3":"CHN","numeric":156,"latitude":35,"longitude":105},{"Name":"Christmas Island","Code":"CX","alpha3":"CXR","numeric":162,"latitude":-10.5,"longitude":105.6667},{"Name":"Cocos (Keeling) Islands","Code":"CC","alpha3":"CCK","numeric":166,"latitude":-12.5,"longitude":96.8333},{"Name":"Colombia","Code":"CO","alpha3":"COL","numeric":170,"latitude":4,"longitude":-72},{"Name":"Comoros","Code":"KM","alpha3":"COM","numeric":174,"latitude":-12.1667,"longitude":44.25},{"Name":"Congo","Code":"CG","alpha3":"COG","numeric":178,"latitude":-1,"longitude":15},{"Name":"Congo, the Democratic Republic of the","Code":"CD","alpha3":"COD","numeric":180,"latitude":0,"longitude":25},{"Name":"Cook Islands","Code":"CK","alpha3":"COK","numeric":184,"latitude":-21.2333,"longitude":-159.7667},{"Name":"Costa Rica","Code":"CR","alpha3":"CRI","numeric":188,"latitude":10,"longitude":-84},{"Name":"Côte d'Ivoire","Code":"CI","alpha3":"CIV","numeric":384,"latitude":8,"longitude":-5},{"Name":"Croatia","Code":"HR","alpha3":"HRV","numeric":191,"latitude":45.1667,"longitude":15.5},{"Name":"Cuba","Code":"CU","alpha3":"CUB","numeric":192,"latitude":21.5,"longitude":-80},{"Name":"Cyprus","Code":"CY","alpha3":"CYP","numeric":196,"latitude":35,"longitude":33},{"Name":"Czech Republic","Code":"CZ","alpha3":"CZE","numeric":203,"latitude":49.75,"longitude":15.5},{"Name":"Denmark","Code":"DK","alpha3":"DNK","numeric":208,"latitude":56,"longitude":10},{"Name":"Djibouti","Code":"DJ","alpha3":"DJI","numeric":262,"latitude":11.5,"longitude":43},{"Name":"Dominica","Code":"DM","alpha3":"DMA","numeric":212,"latitude":15.4167,"longitude":-61.3333},{"Name":"Dominican Republic","Code":"DO","alpha3":"DOM","numeric":214,"latitude":19,"longitude":-70.6667},{"Name":"Ecuador","Code":"EC","alpha3":"ECU","numeric":218,"latitude":-2,"longitude":-77.5},{"Name":"Egypt","Code":"EG","alpha3":"EGY","numeric":818,"latitude":27,"longitude":30},{"Name":"El Salvador","Code":"SV","alpha3":"SLV","numeric":222,"latitude":13.8333,"longitude":-88.9167},{"Name":"Equatorial Guinea","Code":"GQ","alpha3":"GNQ","numeric":226,"latitude":2,"longitude":10},{"Name":"Eritrea","Code":"ER","alpha3":"ERI","numeric":232,"latitude":15,"longitude":39},{"Name":"Estonia","Code":"EE","alpha3":"EST","numeric":233,"latitude":59,"longitude":26},{"Name":"Ethiopia","Code":"ET","alpha3":"ETH","numeric":231,"latitude":8,"longitude":38},{"Name":"Falkland Islands (Malvinas)","Code":"FK","alpha3":"FLK","numeric":238,"latitude":-51.75,"longitude":-59},{"Name":"Faroe Islands","Code":"FO","alpha3":"FRO","numeric":234,"latitude":62,"longitude":-7},{"Name":"Fiji","Code":"FJ","alpha3":"FJI","numeric":242,"latitude":-18,"longitude":175},{"Name":"Finland","Code":"FI","alpha3":"FIN","numeric":246,"latitude":64,"longitude":26},{"Name":"France","Code":"FR","alpha3":"FRA","numeric":250,"latitude":46,"longitude":2},{"Name":"French Guiana","Code":"GF","alpha3":"GUF","numeric":254,"latitude":4,"longitude":-53},{"Name":"French Polynesia","Code":"PF","alpha3":"PYF","numeric":258,"latitude":-15,"longitude":-140},{"Name":"French Southern Territories","Code":"TF","alpha3":"ATF","numeric":260,"latitude":-43,"longitude":67},{"Name":"Gabon","Code":"GA","alpha3":"GAB","numeric":266,"latitude":-1,"longitude":11.75},{"Name":"Gambia","Code":"GM","alpha3":"GMB","numeric":270,"latitude":13.4667,"longitude":-16.5667},{"Name":"Georgia","Code":"GE","alpha3":"GEO","numeric":268,"latitude":42,"longitude":43.5},{"Name":"Germany","Code":"DE","alpha3":"DEU","numeric":276,"latitude":51,"longitude":9},{"Name":"Ghana","Code":"GH","alpha3":"GHA","numeric":288,"latitude":8,"longitude":-2},{"Name":"Gibraltar","Code":"GI","alpha3":"GIB","numeric":292,"latitude":36.1833,"longitude":-5.3667},{"Name":"Greece","Code":"GR","alpha3":"GRC","numeric":300,"latitude":39,"longitude":22},{"Name":"Greenland","Code":"GL","alpha3":"GRL","numeric":304,"latitude":72,"longitude":-40},{"Name":"Grenada","Code":"GD","alpha3":"GRD","numeric":308,"latitude":12.1167,"longitude":-61.6667},{"Name":"Guadeloupe","Code":"GP","alpha3":"GLP","numeric":312,"latitude":16.25,"longitude":-61.5833},{"Name":"Guam","Code":"GU","alpha3":"GUM","numeric":316,"latitude":13.4667,"longitude":144.7833},{"Name":"Guatemala","Code":"GT","alpha3":"GTM","numeric":320,"latitude":15.5,"longitude":-90.25},{"Name":"Guernsey","Code":"GG","alpha3":"GGY","numeric":831,"latitude":49.5,"longitude":-2.56},{"Name":"Guinea","Code":"GN","alpha3":"GIN","numeric":324,"latitude":11,"longitude":-10},{"Name":"Guinea-Bissau","Code":"GW","alpha3":"GNB","numeric":624,"latitude":12,"longitude":-15},{"Name":"Guyana","Code":"GY","alpha3":"GUY","numeric":328,"latitude":5,"longitude":-59},{"Name":"Haiti","Code":"HT","alpha3":"HTI","numeric":332,"latitude":19,"longitude":-72.4167},{"Name":"Heard Island and McDonald Islands","Code":"HM","alpha3":"HMD","numeric":334,"latitude":-53.1,"longitude":72.5167},{"Name":"Holy See (Vatican City State)","Code":"VA","alpha3":"VAT","numeric":336,"latitude":41.9,"longitude":12.45},{"Name":"Honduras","Code":"HN","alpha3":"HND","numeric":340,"latitude":15,"longitude":-86.5},{"Name":"Hong Kong","Code":"HK","alpha3":"HKG","numeric":344,"latitude":22.25,"longitude":114.1667},{"Name":"Hungary","Code":"HU","alpha3":"HUN","numeric":348,"latitude":47,"longitude":20},{"Name":"Iceland","Code":"IS","alpha3":"ISL","numeric":352,"latitude":65,"longitude":-18},{"Name":"India","Code":"IN","alpha3":"IND","numeric":356,"latitude":20,"longitude":77},{"Name":"Indonesia","Code":"ID","alpha3":"IDN","numeric":360,"latitude":-5,"longitude":120},{"Name":"Iran, Islamic Republic of","Code":"IR","alpha3":"IRN","numeric":364,"latitude":32,"longitude":53},{"Name":"Iraq","Code":"IQ","alpha3":"IRQ","numeric":368,"latitude":33,"longitude":44},{"Name":"Ireland","Code":"IE","alpha3":"IRL","numeric":372,"latitude":53,"longitude":-8},{"Name":"Isle of Man","Code":"IM","alpha3":"IMN","numeric":833,"latitude":54.23,"longitude":-4.55},{"Name":"Israel","Code":"IL","alpha3":"ISR","numeric":376,"latitude":31.5,"longitude":34.75},{"Name":"Italy","Code":"IT","alpha3":"ITA","numeric":380,"latitude":42.8333,"longitude":12.8333},{"Name":"Jamaica","Code":"JM","alpha3":"JAM","numeric":388,"latitude":18.25,"longitude":-77.5},{"Name":"Japan","Code":"JP","alpha3":"JPN","numeric":392,"latitude":36,"longitude":138},{"Name":"Jersey","Code":"JE","alpha3":"JEY","numeric":832,"latitude":49.21,"longitude":-2.13},{"Name":"Jordan","Code":"JO","alpha3":"JOR","numeric":400,"latitude":31,"longitude":36},{"Name":"Kazakhstan","Code":"KZ","alpha3":"KAZ","numeric":398,"latitude":48,"longitude":68},{"Name":"Kenya","Code":"KE","alpha3":"KEN","numeric":404,"latitude":1,"longitude":38},{"Name":"Kiribati","Code":"KI","alpha3":"KIR","numeric":296,"latitude":1.4167,"longitude":173},{"Name":"Korea, Democratic People's Republic of","Code":"KP","alpha3":"PRK","numeric":408,"latitude":40,"longitude":127},{"Name":"Korea, Republic of","Code":"KR","alpha3":"KOR","numeric":410,"latitude":37,"longitude":127.5},{"Name":"Kuwait","Code":"KW","alpha3":"KWT","numeric":414,"latitude":29.3375,"longitude":47.6581},{"Name":"Kyrgyzstan","Code":"KG","alpha3":"KGZ","numeric":417,"latitude":41,"longitude":75},{"Name":"Lao People's Democratic Republic","Code":"LA","alpha3":"LAO","numeric":418,"latitude":18,"longitude":105},{"Name":"Latvia","Code":"LV","alpha3":"LVA","numeric":428,"latitude":57,"longitude":25},{"Name":"Lebanon","Code":"LB","alpha3":"LBN","numeric":422,"latitude":33.8333,"longitude":35.8333},{"Name":"Lesotho","Code":"LS","alpha3":"LSO","numeric":426,"latitude":-29.5,"longitude":28.5},{"Name":"Liberia","Code":"LR","alpha3":"LBR","numeric":430,"latitude":6.5,"longitude":-9.5},{"Name":"Libyan Arab Jamahiriya","Code":"LY","alpha3":"LBY","numeric":434,"latitude":25,"longitude":17},{"Name":"Liechtenstein","Code":"LI","alpha3":"LIE","numeric":438,"latitude":47.1667,"longitude":9.5333},{"Name":"Lithuania","Code":"LT","alpha3":"LTU","numeric":440,"latitude":56,"longitude":24},{"Name":"Luxembourg","Code":"LU","alpha3":"LUX","numeric":442,"latitude":49.75,"longitude":6.1667},{"Name":"Macao","Code":"MO","alpha3":"MAC","numeric":446,"latitude":22.1667,"longitude":113.55},{"Name":"Macedonia, the former Yugoslav Republic of","Code":"MK","alpha3":"MKD","numeric":807,"latitude":41.8333,"longitude":22},{"Name":"Madagascar","Code":"MG","alpha3":"MDG","numeric":450,"latitude":-20,"longitude":47},{"Name":"Malawi","Code":"MW","alpha3":"MWI","numeric":454,"latitude":-13.5,"longitude":34},{"Name":"Malaysia","Code":"MY","alpha3":"MYS","numeric":458,"latitude":2.5,"longitude":112.5},{"Name":"Maldives","Code":"MV","alpha3":"MDV","numeric":462,"latitude":3.25,"longitude":73},{"Name":"Mali","Code":"ML","alpha3":"MLI","numeric":466,"latitude":17,"longitude":-4},{"Name":"Malta","Code":"MT","alpha3":"MLT","numeric":470,"latitude":35.8333,"longitude":14.5833},{"Name":"Marshall Islands","Code":"MH","alpha3":"MHL","numeric":584,"latitude":9,"longitude":168},{"Name":"Martinique","Code":"MQ","alpha3":"MTQ","numeric":474,"latitude":14.6667,"longitude":-61},{"Name":"Mauritania","Code":"MR","alpha3":"MRT","numeric":478,"latitude":20,"longitude":-12},{"Name":"Mauritius","Code":"MU","alpha3":"MUS","numeric":480,"latitude":-20.2833,"longitude":57.55},{"Name":"Mayotte","Code":"YT","alpha3":"MYT","numeric":175,"latitude":-12.8333,"longitude":45.1667},{"Name":"Mexico","Code":"MX","alpha3":"MEX","numeric":484,"latitude":23,"longitude":-102},{"Name":"Micronesia, Federated States of","Code":"FM","alpha3":"FSM","numeric":583,"latitude":6.9167,"longitude":158.25},{"Name":"Moldova, Republic of","Code":"MD","alpha3":"MDA","numeric":498,"latitude":47,"longitude":29},{"Name":"Monaco","Code":"MC","alpha3":"MCO","numeric":492,"latitude":43.7333,"longitude":7.4},{"Name":"Mongolia","Code":"MN","alpha3":"MNG","numeric":496,"latitude":46,"longitude":105},{"Name":"Montenegro","Code":"ME","alpha3":"MNE","numeric":499,"latitude":42,"longitude":19},{"Name":"Montserrat","Code":"MS","alpha3":"MSR","numeric":500,"latitude":16.75,"longitude":-62.2},{"Name":"Morocco","Code":"MA","alpha3":"MAR","numeric":504,"latitude":32,"longitude":-5},{"Name":"Mozambique","Code":"MZ","alpha3":"MOZ","numeric":508,"latitude":-18.25,"longitude":35},{"Name":"Myanmar","Code":"MM","alpha3":"MMR","numeric":104,"latitude":22,"longitude":98},{"Name":"Namibia","Code":"NA","alpha3":"NAM","numeric":516,"latitude":-22,"longitude":17},{"Name":"Nauru","Code":"NR","alpha3":"NRU","numeric":520,"latitude":-0.5333,"longitude":166.9167},{"Name":"Nepal","Code":"NP","alpha3":"NPL","numeric":524,"latitude":28,"longitude":84},{"Name":"Netherlands","Code":"NL","alpha3":"NLD","numeric":528,"latitude":52.5,"longitude":5.75},{"Name":"Netherlands Antilles","Code":"AN","alpha3":"ANT","numeric":530,"latitude":12.25,"longitude":-68.75},{"Name":"New Caledonia","Code":"NC","alpha3":"NCL","numeric":540,"latitude":-21.5,"longitude":165.5},{"Name":"New Zealand","Code":"NZ","alpha3":"NZL","numeric":554,"latitude":-41,"longitude":174},{"Name":"Nicaragua","Code":"NI","alpha3":"NIC","numeric":558,"latitude":13,"longitude":-85},{"Name":"Niger","Code":"NE","alpha3":"NER","numeric":562,"latitude":16,"longitude":8},{"Name":"Nigeria","Code":"NG","alpha3":"NGA","numeric":566,"latitude":10,"longitude":8},{"Name":"Niue","Code":"NU","alpha3":"NIU","numeric":570,"latitude":-19.0333,"longitude":-169.8667},{"Name":"Norfolk Island","Code":"NF","alpha3":"NFK","numeric":574,"latitude":-29.0333,"longitude":167.95},{"Name":"Northern Mariana Islands","Code":"MP","alpha3":"MNP","numeric":580,"latitude":15.2,"longitude":145.75},{"Name":"Norway","Code":"NO","alpha3":"NOR","numeric":578,"latitude":62,"longitude":10},{"Name":"Oman","Code":"OM","alpha3":"OMN","numeric":512,"latitude":21,"longitude":57},{"Name":"Pakistan","Code":"PK","alpha3":"PAK","numeric":586,"latitude":30,"longitude":70},{"Name":"Palau","Code":"PW","alpha3":"PLW","numeric":585,"latitude":7.5,"longitude":134.5},{"Name":"Palestinian Territory, Occupied","Code":"PS","alpha3":"PSE","numeric":275,"latitude":32,"longitude":35.25},{"Name":"Panama","Code":"PA","alpha3":"PAN","numeric":591,"latitude":9,"longitude":-80},{"Name":"Papua New Guinea","Code":"PG","alpha3":"PNG","numeric":598,"latitude":-6,"longitude":147},{"Name":"Paraguay","Code":"PY","alpha3":"PRY","numeric":600,"latitude":-23,"longitude":-58},{"Name":"Peru","Code":"PE","alpha3":"PER","numeric":604,"latitude":-10,"longitude":-76},{"Name":"Philippines","Code":"PH","alpha3":"PHL","numeric":608,"latitude":13,"longitude":122},{"Name":"Pitcairn","Code":"PN","alpha3":"PCN","numeric":612,"latitude":-24.7,"longitude":-127.4},{"Name":"Poland","Code":"PL","alpha3":"POL","numeric":616,"latitude":52,"longitude":20},{"Name":"Portugal","Code":"PT","alpha3":"PRT","numeric":620,"latitude":39.5,"longitude":-8},{"Name":"Puerto Rico","Code":"PR","alpha3":"PRI","numeric":630,"latitude":18.25,"longitude":-66.5},{"Name":"Qatar","Code":"QA","alpha3":"QAT","numeric":634,"latitude":25.5,"longitude":51.25},{"Name":"Réunion","Code":"RE","alpha3":"REU","numeric":638,"latitude":-21.1,"longitude":55.6},{"Name":"Romania","Code":"RO","alpha3":"ROU","numeric":642,"latitude":46,"longitude":25},{"Name":"Russian Federation","Code":"RU","alpha3":"RUS","numeric":643,"latitude":60,"longitude":100},{"Name":"Rwanda","Code":"RW","alpha3":"RWA","numeric":646,"latitude":-2,"longitude":30},{"Name":"Saint Helena, Ascension and Tristan da Cunha","Code":"SH","alpha3":"SHN","numeric":654,"latitude":-15.9333,"longitude":-5.7},{"Name":"Saint Kitts and Nevis","Code":"KN","alpha3":"KNA","numeric":659,"latitude":17.3333,"longitude":-62.75},{"Name":"Saint Lucia","Code":"LC","alpha3":"LCA","numeric":662,"latitude":13.8833,"longitude":-61.1333},{"Name":"Saint Pierre and Miquelon","Code":"PM","alpha3":"SPM","numeric":666,"latitude":46.8333,"longitude":-56.3333},{"Name":"Saint Vincent and the Grenadines","Code":"VC","alpha3":"VCT","numeric":670,"latitude":13.25,"longitude":-61.2},{"Name":"Samoa","Code":"WS","alpha3":"WSM","numeric":882,"latitude":-13.5833,"longitude":-172.3333},{"Name":"San Marino","Code":"SM","alpha3":"SMR","numeric":674,"latitude":43.7667,"longitude":12.4167},{"Name":"Sao Tome and Principe","Code":"ST","alpha3":"STP","numeric":678,"latitude":1,"longitude":7},{"Name":"Saudi Arabia","Code":"SA","alpha3":"SAU","numeric":682,"latitude":25,"longitude":45},{"Name":"Senegal","Code":"SN","alpha3":"SEN","numeric":686,"latitude":14,"longitude":-14},{"Name":"Serbia","Code":"RS","alpha3":"SRB","numeric":688,"latitude":44,"longitude":21},{"Name":"Seychelles","Code":"SC","alpha3":"SYC","numeric":690,"latitude":-4.5833,"longitude":55.6667},{"Name":"Sierra Leone","Code":"SL","alpha3":"SLE","numeric":694,"latitude":8.5,"longitude":-11.5},{"Name":"Singapore","Code":"SG","alpha3":"SGP","numeric":702,"latitude":1.3667,"longitude":103.8},{"Name":"Slovakia","Code":"SK","alpha3":"SVK","numeric":703,"latitude":48.6667,"longitude":19.5},{"Name":"Slovenia","Code":"SI","alpha3":"SVN","numeric":705,"latitude":46,"longitude":15},{"Name":"Solomon Islands","Code":"SB","alpha3":"SLB","numeric":90,"latitude":-8,"longitude":159},{"Name":"Somalia","Code":"SO","alpha3":"SOM","numeric":706,"latitude":10,"longitude":49},{"Name":"South Africa","Code":"ZA","alpha3":"ZAF","numeric":710,"latitude":-29,"longitude":24},{"Name":"South Georgia and the South Sandwich Islands","Code":"GS","alpha3":"SGS","numeric":239,"latitude":-54.5,"longitude":-37},{"Name":"Spain","Code":"ES","alpha3":"ESP","numeric":724,"latitude":40,"longitude":-4},{"Name":"Sri Lanka","Code":"LK","alpha3":"LKA","numeric":144,"latitude":7,"longitude":81},{"Name":"Sudan","Code":"SD","alpha3":"SDN","numeric":736,"latitude":15,"longitude":30},{"Name":"Suriname","Code":"SR","alpha3":"SUR","numeric":740,"latitude":4,"longitude":-56},{"Name":"Svalbard and Jan Mayen","Code":"SJ","alpha3":"SJM","numeric":744,"latitude":78,"longitude":20},{"Name":"Swaziland","Code":"SZ","alpha3":"SWZ","numeric":748,"latitude":-26.5,"longitude":31.5},{"Name":"Sweden","Code":"SE","alpha3":"SWE","numeric":752,"latitude":62,"longitude":15},{"Name":"Switzerland","Code":"CH","alpha3":"CHE","numeric":756,"latitude":47,"longitude":8},{"Name":"Syrian Arab Republic","Code":"SY","alpha3":"SYR","numeric":760,"latitude":35,"longitude":38},{"Name":"Taiwan, Province of China","Code":"TW","alpha3":"TWN","numeric":158,"latitude":23.5,"longitude":121},{"Name":"Tajikistan","Code":"TJ","alpha3":"TJK","numeric":762,"latitude":39,"longitude":71},{"Name":"Tanzania, United Republic of","Code":"TZ","alpha3":"TZA","numeric":834,"latitude":-6,"longitude":35},{"Name":"Thailand","Code":"TH","alpha3":"THA","numeric":764,"latitude":15,"longitude":100},{"Name":"Timor-Leste","Code":"TL","alpha3":"TLS","numeric":626,"latitude":-8.55,"longitude":125.5167},{"Name":"Togo","Code":"TG","alpha3":"TGO","numeric":768,"latitude":8,"longitude":1.1667},{"Name":"Tokelau","Code":"TK","alpha3":"TKL","numeric":772,"latitude":-9,"longitude":-172},{"Name":"Tonga","Code":"TO","alpha3":"TON","numeric":776,"latitude":-20,"longitude":-175},{"Name":"Trinidad and Tobago","Code":"TT","alpha3":"TTO","numeric":780,"latitude":11,"longitude":-61},{"Name":"Tunisia","Code":"TN","alpha3":"TUN","numeric":788,"latitude":34,"longitude":9},{"Name":"Turkey","Code":"TR","alpha3":"TUR","numeric":792,"latitude":39,"longitude":35},{"Name":"Turkmenistan","Code":"TM","alpha3":"TKM","numeric":795,"latitude":40,"longitude":60},{"Name":"Turks and Caicos Islands","Code":"TC","alpha3":"TCA","numeric":796,"latitude":21.75,"longitude":-71.5833},{"Name":"Tuvalu","Code":"TV","alpha3":"TUV","numeric":798,"latitude":-8,"longitude":178},{"Name":"Uganda","Code":"UG","alpha3":"UGA","numeric":800,"latitude":1,"longitude":32},{"Name":"Ukraine","Code":"UA","alpha3":"UKR","numeric":804,"latitude":49,"longitude":32},{"Name":"United Arab Emirates","Code":"AE","alpha3":"ARE","numeric":784,"latitude":24,"longitude":54},{"Name":"United Kingdom","Code":"GB","alpha3":"GBR","numeric":826,"latitude":54,"longitude":-2},{"Name":"United States","Code":"US","alpha3":"USA","numeric":840,"latitude":38,"longitude":-97},{"Name":"United States Minor Outlying Islands","Code":"UM","alpha3":"UMI","numeric":581,"latitude":19.2833,"longitude":166.6},{"Name":"Uruguay","Code":"UY","alpha3":"URY","numeric":858,"latitude":-33,"longitude":-56},{"Name":"Uzbekistan","Code":"UZ","alpha3":"UZB","numeric":860,"latitude":41,"longitude":64},{"Name":"Vanuatu","Code":"VU","alpha3":"VUT","numeric":548,"latitude":-16,"longitude":167},{"Name":"Venezuela, Bolivarian Republic of","Code":"VE","alpha3":"VEN","numeric":862,"latitude":8,"longitude":-66},{"Name":"Viet Nam","Code":"VN","alpha3":"VNM","numeric":704,"latitude":16,"longitude":106},{"Name":"Virgin Islands, British","Code":"VG","alpha3":"VGB","numeric":92,"latitude":18.5,"longitude":-64.5},{"Name":"Virgin Islands, U.S.","Code":"VI","alpha3":"VIR","numeric":850,"latitude":18.3333,"longitude":-64.8333},{"Name":"Wallis and Futuna","Code":"WF","alpha3":"WLF","numeric":876,"latitude":-13.3,"longitude":-176.2},{"Name":"Western Sahara","Code":"EH","alpha3":"ESH","numeric":732,"latitude":24.5,"longitude":-13},{"Name":"Yemen","Code":"YE","alpha3":"YEM","numeric":887,"latitude":15,"longitude":48},{"Name":"Zambia","Code":"ZM","alpha3":"ZMB","numeric":894,"latitude":-15,"longitude":30},{"Name":"Zimbabwe","Code":"ZW","alpha3":"ZWE","numeric":716,"latitude":-20,"longitude":30},{"Name":"Afghanistan","Code":"AF","alpha3":"AFG","numeric":4,"latitude":33,"longitude":65}];

const date = new Date().toISOString().slice(0, 10).replace(/-/g, "");

const arng = new alea(date);
const rand = Math.ceil(arng.quick() * country_codes.length - 1);

const country_code = computed(() => {
  return country_codes[rand].Code.toLowerCase();
});

const country_name = computed(() => {
  return country_codes[rand].Name;
});


const guesses = ref([]);

const guesses_remaining = ref(4);

const checkIfAlreadyGuessed = () => {
  return Array.from(guesses.value).includes(currentGuess.value);
}

const lookupCountry = (code) => {
  if(!code) return;
  return country_codes.find((country) => country.Code === code).Name;
};

const bearing = ref(null);
const distance = ref(null);

const addGuess = () => {

  if (checkIfAlreadyGuessed()) {
    alert('You already guessed that one!');
    return;
  }

  guesses.value.push(currentGuess.value);
  if (currentGuess.value.toLowerCase() === country_code.value) {
    guesses_remaining.value = 0;
    userWon.value = true;
  }

  if (guesses_remaining.value > 0) {
    guesses_remaining.value--;
    const { latitude, longitude} = country_codes.find(country => country.Code.toLowerCase() === country_code.value.toLowerCase());
    const guessedCountry = country_codes.find(country => country.Code.toLowerCase() === currentGuess.value.toLowerCase());

    const guessedLat = guessedCountry.latitude;
    const guessedLong = guessedCountry.longitude;

    bearing.value = getBearing(
      new Position(guessedLat, guessedLong),
      new Position(latitude, longitude)
    );

    distance.value = getDistance(
      new Position(guessedLat, guessedLong),
      new Position(latitude, longitude)
    ) * 1.15078;
  }

  currentGuess.value = '';
};

const currentGuess = ref();
const userWon = ref(false);
</script>

<template>
  <main>
    <div class="flex flex-col align-center justify-bottom">

      <h1 class="mb-8 text-center text-5xl">Today's Flag</h1>
      <img
        :src="`https://flagcdn.com/${country_code}.svg`"
        width="300"
      >
      <div
        class="py-2 mt-4"
        type="info"
        v-if="!userWon && guesses_remaining > 0"
      >Guesses Remaining: {{ guesses_remaining }}</div>

      <p
        class="py-2 mt-4 text-4xl text-center"
        v-if="userWon"
      >You won!</p>

      <div
        class="py-2 mt-4 text-2xl text-center"
        type="info"
        v-if="guesses_remaining === 0 || userWon"
      >The <span v-if="!userWon">correct</span> answer was... {{ country_name }}!<br> <br>Pop back tomorrow for another
        flag!</div>
    </div>
    <div
      class="flex mt-8 flex-col"
      v-if="guesses_remaining > 0"
    >
      <v-divider></v-divider>
      <v-autocomplete
        v-if="!userWon"
        v-model="currentGuess"
        item-value="Code"
        item-title="Name"
        clearable
        label="Type your guess here..."
        :items="country_codes"
      ></v-autocomplete>
      <v-btn
        color="primary"
        class="mb-4"
        @click="addGuess"
        v-if="!userWon"
      >Guess</v-btn>

      <v-alert color="blue" class="mt-4 mb-8" v-if="distance > 0">
        Hint! The country is to the {{ getCardinal(Math.round(bearing)) }} and is {{ Math.round(distance) }} miles from your last guess!
      </v-alert>

      <div
        v-if="!userWon && guesses && guesses[0]"
        class="text-black bg-slate-200 mb-4 p-2 border-solid border-1 border-slate-200"
        ref="guess-1"
      >{{ lookupCountry(guesses[0]) }}</div>
      <div
        v-if="!userWon  && guesses && guesses[1]"
        class="text-black bg-slate-200 mb-4 p-2 border-solid border-1 border-slate-200"
      ref="guess-2"
    >{{ lookupCountry(guesses[1]) }}</div>
    <div
      v-if="!userWon && guesses && guesses[2]"
      class="text-black bg-slate-200 mb-4 p-2 border-solid border-1 border-slate-200"
      ref="guess-3"
    >{{ lookupCountry(guesses[2]) }}</div>
    <div
      v-if="!userWon  && guesses && guesses[3]"
      class="text-black bg-slate-200 mb-4 p-2 border-solid border-1 border-slate-200"
      ref="guess-4"
    >{{ lookupCountry(guesses[3]) }}</div>
  </div>
</main></template>
