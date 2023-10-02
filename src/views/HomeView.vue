<script setup>
import { computed, ref } from 'vue';

const date = new Date().toISOString().slice(0, 10).replace(/-/g, "");

const arng = new alea(date);
const rand = Math.ceil(arng.quick() * 259);

const country_code = computed(() => {
  return country_codes[rand].Code.toLowerCase();
});

const country_name = computed(() => {
  return country_codes[rand].Name;
});

const country_codes = [{ "Code": "AF", "Name": "Afghanistan" }, { "Code": "AX", "Name": "\u00c5land Islands" }, { "Code": "AL", "Name": "Albania" }, { "Code": "DZ", "Name": "Algeria" }, { "Code": "AS", "Name": "American Samoa" }, { "Code": "AD", "Name": "Andorra" }, { "Code": "AO", "Name": "Angola" }, { "Code": "AI", "Name": "Anguilla" }, { "Code": "AQ", "Name": "Antarctica" }, { "Code": "AG", "Name": "Antigua and Barbuda" }, { "Code": "AR", "Name": "Argentina" }, { "Code": "AM", "Name": "Armenia" }, { "Code": "AW", "Name": "Aruba" }, { "Code": "AU", "Name": "Australia" }, { "Code": "AT", "Name": "Austria" }, { "Code": "AZ", "Name": "Azerbaijan" }, { "Code": "BS", "Name": "Bahamas" }, { "Code": "BH", "Name": "Bahrain" }, { "Code": "BD", "Name": "Bangladesh" }, { "Code": "BB", "Name": "Barbados" }, { "Code": "BY", "Name": "Belarus" }, { "Code": "BE", "Name": "Belgium" }, { "Code": "BZ", "Name": "Belize" }, { "Code": "BJ", "Name": "Benin" }, { "Code": "BM", "Name": "Bermuda" }, { "Code": "BT", "Name": "Bhutan" }, { "Code": "BO", "Name": "Bolivia, Plurinational State of" }, { "Code": "BQ", "Name": "Bonaire, Sint Eustatius and Saba" }, { "Code": "BA", "Name": "Bosnia and Herzegovina" }, { "Code": "BW", "Name": "Botswana" }, { "Code": "BV", "Name": "Bouvet Island" }, { "Code": "BR", "Name": "Brazil" }, { "Code": "IO", "Name": "British Indian Ocean Territory" }, { "Code": "BN", "Name": "Brunei Darussalam" }, { "Code": "BG", "Name": "Bulgaria" }, { "Code": "BF", "Name": "Burkina Faso" }, { "Code": "BI", "Name": "Burundi" }, { "Code": "KH", "Name": "Cambodia" }, { "Code": "CM", "Name": "Cameroon" }, { "Code": "CA", "Name": "Canada" }, { "Code": "CV", "Name": "Cape Verde" }, { "Code": "KY", "Name": "Cayman Islands" }, { "Code": "CF", "Name": "Central African Republic" }, { "Code": "TD", "Name": "Chad" }, { "Code": "CL", "Name": "Chile" }, { "Code": "CN", "Name": "China" }, { "Code": "CX", "Name": "Christmas Island" }, { "Code": "CC", "Name": "Cocos (Keeling) Islands" }, { "Code": "CO", "Name": "Colombia" }, { "Code": "KM", "Name": "Comoros" }, { "Code": "CG", "Name": "Congo" }, { "Code": "CD", "Name": "Congo, the Democratic Republic of the" }, { "Code": "CK", "Name": "Cook Islands" }, { "Code": "CR", "Name": "Costa Rica" }, { "Code": "CI", "Name": "C\u00f4te d'Ivoire" }, { "Code": "HR", "Name": "Croatia" }, { "Code": "CU", "Name": "Cuba" }, { "Code": "CW", "Name": "Cura\u00e7ao" }, { "Code": "CY", "Name": "Cyprus" }, { "Code": "CZ", "Name": "Czech Republic" }, { "Code": "DK", "Name": "Denmark" }, { "Code": "DJ", "Name": "Djibouti" }, { "Code": "DM", "Name": "Dominica" }, { "Code": "DO", "Name": "Dominican Republic" }, { "Code": "EC", "Name": "Ecuador" }, { "Code": "EG", "Name": "Egypt" }, { "Code": "SV", "Name": "El Salvador" }, { "Code": "GQ", "Name": "Equatorial Guinea" }, { "Code": "ER", "Name": "Eritrea" }, { "Code": "EE", "Name": "Estonia" }, { "Code": "ET", "Name": "Ethiopia" }, { "Code": "FK", "Name": "Falkland Islands (Malvinas)" }, { "Code": "FO", "Name": "Faroe Islands" }, { "Code": "FJ", "Name": "Fiji" }, { "Code": "FI", "Name": "Finland" }, { "Code": "FR", "Name": "France" }, { "Code": "GF", "Name": "French Guiana" }, { "Code": "PF", "Name": "French Polynesia" }, { "Code": "TF", "Name": "French Southern Territories" }, { "Code": "GA", "Name": "Gabon" }, { "Code": "GM", "Name": "Gambia" }, { "Code": "GE", "Name": "Georgia" }, { "Code": "DE", "Name": "Germany" }, { "Code": "GH", "Name": "Ghana" }, { "Code": "GI", "Name": "Gibraltar" }, { "Code": "GR", "Name": "Greece" }, { "Code": "GL", "Name": "Greenland" }, { "Code": "GD", "Name": "Grenada" }, { "Code": "GP", "Name": "Guadeloupe" }, { "Code": "GU", "Name": "Guam" }, { "Code": "GT", "Name": "Guatemala" }, { "Code": "GG", "Name": "Guernsey" }, { "Code": "GN", "Name": "Guinea" }, { "Code": "GW", "Name": "Guinea-Bissau" }, { "Code": "GY", "Name": "Guyana" }, { "Code": "HT", "Name": "Haiti" }, { "Code": "HM", "Name": "Heard Island and McDonald Islands" }, { "Code": "VA", "Name": "Holy See (Vatican City State)" }, { "Code": "HN", "Name": "Honduras" }, { "Code": "HK", "Name": "Hong Kong" }, { "Code": "HU", "Name": "Hungary" }, { "Code": "IS", "Name": "Iceland" }, { "Code": "IN", "Name": "India" }, { "Code": "ID", "Name": "Indonesia" }, { "Code": "IR", "Name": "Iran, Islamic Republic of" }, { "Code": "IQ", "Name": "Iraq" }, { "Code": "IE", "Name": "Ireland" }, { "Code": "IM", "Name": "Isle of Man" }, { "Code": "IL", "Name": "Israel" }, { "Code": "IT", "Name": "Italy" }, { "Code": "JM", "Name": "Jamaica" }, { "Code": "JP", "Name": "Japan" }, { "Code": "JE", "Name": "Jersey" }, { "Code": "JO", "Name": "Jordan" }, { "Code": "KZ", "Name": "Kazakhstan" }, { "Code": "KE", "Name": "Kenya" }, { "Code": "KI", "Name": "Kiribati" }, { "Code": "KP", "Name": "Korea, Democratic People's Republic of" }, { "Code": "KR", "Name": "Korea, Republic of" }, { "Code": "KW", "Name": "Kuwait" }, { "Code": "KG", "Name": "Kyrgyzstan" }, { "Code": "LA", "Name": "Lao People's Democratic Republic" }, { "Code": "LV", "Name": "Latvia" }, { "Code": "LB", "Name": "Lebanon" }, { "Code": "LS", "Name": "Lesotho" }, { "Code": "LR", "Name": "Liberia" }, { "Code": "LY", "Name": "Libya" }, { "Code": "LI", "Name": "Liechtenstein" }, { "Code": "LT", "Name": "Lithuania" }, { "Code": "LU", "Name": "Luxembourg" }, { "Code": "MO", "Name": "Macao" }, { "Code": "MK", "Name": "Macedonia, the Former Yugoslav Republic of" }, { "Code": "MG", "Name": "Madagascar" }, { "Code": "MW", "Name": "Malawi" }, { "Code": "MY", "Name": "Malaysia" }, { "Code": "MV", "Name": "Maldives" }, { "Code": "ML", "Name": "Mali" }, { "Code": "MT", "Name": "Malta" }, { "Code": "MH", "Name": "Marshall Islands" }, { "Code": "MQ", "Name": "Martinique" }, { "Code": "MR", "Name": "Mauritania" }, { "Code": "MU", "Name": "Mauritius" }, { "Code": "YT", "Name": "Mayotte" }, { "Code": "MX", "Name": "Mexico" }, { "Code": "FM", "Name": "Micronesia, Federated States of" }, { "Code": "MD", "Name": "Moldova, Republic of" }, { "Code": "MC", "Name": "Monaco" }, { "Code": "MN", "Name": "Mongolia" }, { "Code": "ME", "Name": "Montenegro" }, { "Code": "MS", "Name": "Montserrat" }, { "Code": "MA", "Name": "Morocco" }, { "Code": "MZ", "Name": "Mozambique" }, { "Code": "MM", "Name": "Myanmar" }, { "Code": "NA", "Name": "Namibia" }, { "Code": "NR", "Name": "Nauru" }, { "Code": "NP", "Name": "Nepal" }, { "Code": "NL", "Name": "Netherlands" }, { "Code": "NC", "Name": "New Caledonia" }, { "Code": "NZ", "Name": "New Zealand" }, { "Code": "NI", "Name": "Nicaragua" }, { "Code": "NE", "Name": "Niger" }, { "Code": "NG", "Name": "Nigeria" }, { "Code": "NU", "Name": "Niue" }, { "Code": "NF", "Name": "Norfolk Island" }, { "Code": "MP", "Name": "Northern Mariana Islands" }, { "Code": "NO", "Name": "Norway" }, { "Code": "OM", "Name": "Oman" }, { "Code": "PK", "Name": "Pakistan" }, { "Code": "PW", "Name": "Palau" }, { "Code": "PS", "Name": "Palestine, State of" }, { "Code": "PA", "Name": "Panama" }, { "Code": "PG", "Name": "Papua New Guinea" }, { "Code": "PY", "Name": "Paraguay" }, { "Code": "PE", "Name": "Peru" }, { "Code": "PH", "Name": "Philippines" }, { "Code": "PN", "Name": "Pitcairn" }, { "Code": "PL", "Name": "Poland" }, { "Code": "PT", "Name": "Portugal" }, { "Code": "PR", "Name": "Puerto Rico" }, { "Code": "QA", "Name": "Qatar" }, { "Code": "RE", "Name": "R\u00e9union" }, { "Code": "RO", "Name": "Romania" }, { "Code": "RU", "Name": "Russian Federation" }, { "Code": "RW", "Name": "Rwanda" }, { "Code": "BL", "Name": "Saint Barth\u00e9lemy" }, { "Code": "SH", "Name": "Saint Helena, Ascension and Tristan da Cunha" }, { "Code": "KN", "Name": "Saint Kitts and Nevis" }, { "Code": "LC", "Name": "Saint Lucia" }, { "Code": "MF", "Name": "Saint Martin (French part)" }, { "Code": "PM", "Name": "Saint Pierre and Miquelon" }, { "Code": "VC", "Name": "Saint Vincent and the Grenadines" }, { "Code": "WS", "Name": "Samoa" }, { "Code": "SM", "Name": "San Marino" }, { "Code": "ST", "Name": "Sao Tome and Principe" }, { "Code": "SA", "Name": "Saudi Arabia" }, { "Code": "SN", "Name": "Senegal" }, { "Code": "RS", "Name": "Serbia" }, { "Code": "SC", "Name": "Seychelles" }, { "Code": "SL", "Name": "Sierra Leone" }, { "Code": "SG", "Name": "Singapore" }, { "Code": "SX", "Name": "Sint Maarten (Dutch part)" }, { "Code": "SK", "Name": "Slovakia" }, { "Code": "SI", "Name": "Slovenia" }, { "Code": "SB", "Name": "Solomon Islands" }, { "Code": "SO", "Name": "Somalia" }, { "Code": "ZA", "Name": "South Africa" }, { "Code": "GS", "Name": "South Georgia and the South Sandwich Islands" }, { "Code": "SS", "Name": "South Sudan" }, { "Code": "ES", "Name": "Spain" }, { "Code": "LK", "Name": "Sri Lanka" }, { "Code": "SD", "Name": "Sudan" }, { "Code": "SR", "Name": "Suriname" }, { "Code": "SJ", "Name": "Svalbard and Jan Mayen" }, { "Code": "SZ", "Name": "Swaziland" }, { "Code": "SE", "Name": "Sweden" }, { "Code": "CH", "Name": "Switzerland" }, { "Code": "SY", "Name": "Syrian Arab Republic" }, { "Code": "TW", "Name": "Taiwan, Province of China" }, { "Code": "TJ", "Name": "Tajikistan" }, { "Code": "TZ", "Name": "Tanzania, United Republic of" }, { "Code": "TH", "Name": "Thailand" }, { "Code": "TL", "Name": "Timor-Leste" }, { "Code": "TG", "Name": "Togo" }, { "Code": "TK", "Name": "Tokelau" }, { "Code": "TO", "Name": "Tonga" }, { "Code": "TT", "Name": "Trinidad and Tobago" }, { "Code": "TN", "Name": "Tunisia" }, { "Code": "TR", "Name": "Turkey" }, { "Code": "TM", "Name": "Turkmenistan" }, { "Code": "TC", "Name": "Turks and Caicos Islands" }, { "Code": "TV", "Name": "Tuvalu" }, { "Code": "UG", "Name": "Uganda" }, { "Code": "UA", "Name": "Ukraine" }, { "Code": "AE", "Name": "United Arab Emirates" }, { "Code": "GB", "Name": "United Kingdom" }, { "Code": "US", "Name": "United States" }, { "Code": "UM", "Name": "United States Minor Outlying Islands" }, { "Code": "UY", "Name": "Uruguay" }, { "Code": "UZ", "Name": "Uzbekistan" }, { "Code": "VU", "Name": "Vanuatu" }, { "Code": "VE", "Name": "Venezuela, Bolivarian Republic of" }, { "Code": "VN", "Name": "Viet Nam" }, { "Code": "VG", "Name": "Virgin Islands, British" }, { "Code": "VI", "Name": "Virgin Islands, U.S." }, { "Code": "WF", "Name": "Wallis and Futuna" }, { "Code": "EH", "Name": "Western Sahara" }, { "Code": "YE", "Name": "Yemen" }, { "Code": "ZM", "Name": "Zambia" }, { "Code": "ZW", "Name": "Zimbabwe" }];

const guesses = ref([]);

const guesses_remaining = ref(4);

const checkIfAlreadyGuessed = () => {
  return Array.from(guesses.value).includes(currentGuess.value);
}

const lookupCountry = (code) => {
  return country_codes.find((country) => country.Code === code).Name;
};

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
