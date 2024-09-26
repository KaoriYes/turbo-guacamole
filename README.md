# turbo-guacamole


## Proces
Ik wilde graag een manier vinden om mijn foto's die ik door Europa had gemaakt, met interactie te tonen.
### Week 1: Idee uitwerken
Ik ben begonnen met ideeën verzinnen om de kaart van Europa op een interactieve manier te maken, met alleen CSS. Daarbij had ik initieel het idee om de landen van Europa in een grid systeem te zetten, hierbij wilde ik elk land uitsnijden met gebruik van een mask-image, hier had ik al eerder mee gewerkt [portfolio Quinten Kok](https://quintenkok.nl). Verder wilde ik de interactie maken met behulp van de :has selector om zo een diepte te creëren. Als over je een land, of de grid vlakken, heen gaat met de muis, zou het land een diepte of scale effect krijgen. Dit leek me tof omdat normale kaarten ook in een 'gridsysteem' uitgelijnd worden. Zie idee foto
[grid-system-europe](./grid-system-europe.jpg).


Dit bleek in praktijk toch minder goed te werken, ik vond het lastig om de scaling goed te krijgen in mijn handschetsen en heb die maar snel weggedaan. Toen ben ik meer gaan kijken hoe we een diepte effect konden creëren in een kaart. Ik had het met Tristan tijdens de eindopdracht veel over de nieuwe 3D potentie van het web en dat er veel vraag naar was, hierdoor ben ik die kant op gaan kijken. Een 3D variant van een kaart is een wereldbol. Toen ben ik hiermee gaan spelen, hoe ga ik een kaart van Europa laten zien op een ronde vorm, uiteindelijk ben ik geland op het idee was ik bedacht had heb ik onderzoek gedaan naar een wereld bol in CSS. Dit leek op verschillende manieren gedaan te worden. Ik vond een aantal verschillende varianten, zie hieronder:
[https://codepen.io/itsnickraus/pen/KwEqWO](https://codepen.io/itsnickraus/pen/KwEqWO)
[https://stackoverflow.com/questions/27781634/rotating-globe-in-css](https://stackoverflow.com/questions/27781634/rotating-globe-in-css)
[https://codepen.io/jperals/pen/ExYqwyJ?editors=1100](https://codepen.io/jperals/pen/ExYqwyJ?editors=1100)


Mijn idee van een wereldbol begon wel ergens op te lijken, ik had nu ook een idee in mijn hoofd voor hoe gedetailleerd ik het wil. De laatste link was bijvoorbeeld nagenoeg wat ik wil, maar hiervoor is zo veel HTML en CSS geschreven, dat ik het niet meer leesbaar of tijd-nuttig vond, ik had officieel maar twee weken (heb wel iets meer tijd nodig gehad als je het onderzoeksproces meerekent, ik ben ruim twee en een halve week bezig geweest met code). 

Uiteindelijk ben ik op het design volgende design gekomen van de wereldbol: 
[Wereldbol-design](./Wereldbol-design.png)

Hiermee wilde ik het idee van 3D nabootsen. Ik had al een beetje gespeeld met mask-images, dus ik ben toen op zoek gegaan naar mooie en nette SVG's van landen in Europa. Hierin wilde ik de landen om de wereldbol heen laten draaien om een 3D effect te maken. 

### Week 2: In de code duiken met het definitieve idee

Toen ik dit eenmaal had, ging ik aan de slag met de wereldbol, hiervoor heb ik een aantal designs en ideetjes gevonden, waaronder eentje van Sanne (de box-shadow voor de bol). Erg handig, dat scheelde weer een half uurtje. 

			* {
		--size-globe: 40vw;
		--color-shade-gray: #1212120a  ;
		--size-translate: calc(var(--size-globe) * .4999);
		}
	:global(body) {
		height: 500vh !important;
		}
	
	section.globe{
		position: fixed;
		top: calc(50vh - (var(--size-globe) / 2));
		left: calc(50vw - (var(--size-globe) / 2));
		height: var(--size-globe);
		width: var(--size-globe);
		background-color: var(--bg-color);
		border-radius: 50%;
		box-shadow:
			calc(var(--size-globe)* .0025) calc(var(--size-globe)* .0025) calc(var(--size-globe)* .025) calc(var(--size-globe)* .025) #fff,
			-2.5vw -1vw 0 0 rgba(50, 50, 50, 1) inset,
			-20vw -30vw 0 0 rgba(0, 0, 0, .3) inset,
			-5vw -2.5vw 0 0 rgba(50, 50, 50, .8) inset
	;
		perspective-origin: bottom;
		transform-style: preserve-3d;
		z-index: 99;
		
		}


Dit is de code van de Wereld bol. 

> Note: Ik heb veel gebruik gemaakt van classes in het proces om meer overzicht te behouden, deze waren (en zijn nu) niet perse nodig. Maar om mijn warrigheid te verminderen heb ik veel zo gewerkt. Ook zal je wat Sveltekit code zien, dat komt omdat dit ook te zien is in mijn portfolio. Dit vond ik leuk om te verwerken, het blijft alleen html en CSS, maar dan in 1 bestand -- tevens voor overzicht. Dit is allemaal weggehaald voor het finale product


De code voor de landen:

		section.country {
		width: 100%;
		height: 100%;
		position: absolute;
		left: 0;
		top: 0;
		cursor: pointer;
		background-color: var(--bg-color-circle);
		-webkit-mask-position: 50% ;
		background-repeat: no-repeat;
		-webkit-mask-repeat: no-repeat;
		animation: globe-scroll linear forwards;
		animation-range: contain;
		animation-timeline: scroll(root block);
		backface-visibility: inherit;
		transform-origin: 50%;
		}
	
		.NL{
			mask-image: url('/SVG/NL.svg');
			
			-webkit-mask-size: 15%;
			--translate-animation:  rotateY(0) scale3d(.75, 1, 1)  translate3d(10%, -50%, var(--size-translate)) ;
			--translate-animation-end: scale3d(.75, 1, .75) translate3d(10%, -50%, var(--size-translate)) ;
			--rotate-3d-animation: rotate3d(-.5, 1, .1, 720deg) ;
			}
		.Spain{
			mask-image: url('/SVG/Spain.svg');
			-webkit-mask-size: 30%;
			--translate-animation: rotateY(0) translate3d(0, 0, var(--size-translate));
			--translate-animation-end: translate3d(0, 0, var(--size-translate));
			--rotate-3d-animation: rotate3d(-.5, 1, .1, 720deg);
			}
	

Ik heb vooral geprobeerd om met variabeles te werken, zodat ik meer landen kon toevoegen wanneer ze klaar waren.

			
	@keyframes globe-scroll {
		from{
			transform: var(--translate-animation);
			}
		to{
			transform: var(--rotate-3d-animation) var(--translate-animation-end) ;
			}
		}

En hier de initiele animatie


#### problemen hiermee

Omdat de landen niet gepositioneerd werden, heb ik er daar meer aan toe gevoegd, maar ik had ook het probleem dat de landen te vroeg om de wereldbol heen gingen en hiermee niet helemaal soepel liepen.


### Nieuwe versie

		section.globe{
		position: fixed;
		top: calc(50vh - (var(--size-globe) / 2));
		left: calc(50vw - (var(--size-globe) / 2));
		height: var(--size-globe);
		width: var(--size-globe);
		background-color: var(--bg-color);
		border-radius: 50%;
		box-shadow:
			calc(var(--size-globe)* .0025) calc(var(--size-globe)* .0025) calc(var(--size-globe)* .025) calc(var(--size-globe)* .025) #fff,
			-2.5vw -1vw 0 0 rgba(50, 50, 50, 1) inset,
			-20vw -30vw 0 0 rgba(0, 0, 0, .3) inset,
			-5vw -2.5vw 0 0 rgba(50, 50, 50, .8) inset
	;
		perspective-origin: bottom;
		transform-style: preserve-3d;
		z-index: 99;
		
		}
	section.country {
		position: absolute;
		cursor: pointer;
		background-color: var(--bg-color-circle);
		-webkit-mask-size: 100%;
		-webkit-mask-position: 50% ;
		background-repeat: no-repeat;
		-webkit-mask-repeat: no-repeat;
		animation: globe-scroll linear forwards;
		animation-range: contain;
		animation-timeline: scroll(root block);
		backface-visibility: inherit;
		transform-origin: 50%;
		
		}
	
		.NL{
			mask-image: url('/SVG/NL.svg');
			transform-origin: top;
			--size-country: 15%;
			width: var(--size-country);
			height: var(--size-country);
			left: calc(50% - (var(--size-country) / 2) + 10%);
			top: calc(50% - (var(--size-country) / 2) - 25%);
			--size-translate: calc((var(--size-globe) * .4999) + (var(--size-globe) * .15));
			--translate-animation:  rotateY(0) scale3d(.75, 1, .8)  translate3d(0, 0, var(--size-translate)) ;
			--translate-animation-end: scale3d(.75, 1, .8) translate3d(0, 0, var(--size-translate)) ;
			--rotate-3d-animation: rotate3d(-.5, 1, .1, 720deg) ;
			}
		.Spain{
			mask-image: url('/SVG/Spain.svg');
			--size-country: 30%;
			width: var(--size-country);
			height: var(--size-country);
			left: calc(50% - (var(--size-country) / 2));
			top: calc(50% - (var(--size-country) / 2));
			--translate-animation: rotateY(0) translate3d(0, 0, var(--size-translate));
			--translate-animation-end: translate3d(0, 0, var(--size-translate));
			--rotate-3d-animation: rotate3d(-.5, 1, .1, 720deg);
			}
	
	
	@keyframes globe-scroll {
		from{
			transform: var(--translate-animation);
			}
		to{
			transform: var(--rotate-3d-animation) var(--translate-animation-end) ;
			}
		}




Nu leek de wereldbol iets beter te werken, ging ik vanuit hier door om meer landen toe te voegen. Dat maakte het niet makkelijker, hoe moet je midden in een 3d animatie beginnen?? 


			section.Germany{
			mask-image: url('/SVG/Germany.svg');
			--size-country: 30%;
			--size-translate: calc((var(--size-globe) * .4999) + (var(--size-globe) * .05));
			--translate-animation: rotateY(0) translate3d(80%, -33%, var(--size-translate));
			--translate-animation-end:  translate3d(90%, -33%, var(--size-translate));
			scroll-margin: 50vh;
			}
	section.Estonia{
		mask-image: url('/SVG/Estonia.svg');
		--size-country: 20%;
		rotate:  .5 -1 .25 180deg;
		height: calc(var(--size-country) * 1.1);
		--size-translate: calc((var(--size-globe) * .4999) + (var(--size-globe) * .17));
		--translate-animation:  rotateY(0) scale3d(.75, 1, .8)  translate3d(-100%, -100%, var(--size-translate)) ;
		--translate-animation-end: scale3d(.75, 1, .8) translate3d(-100%, -100%, var(--size-translate)) ;
		}



Dit had ik uiteindelijk opgelost door de landen (in dit geval Estland) een extra rotate te geven, ze begonnen niet later, maar hadden nu een andere startpositie. Dit maakt het niet perse makkelijker, maar het was een begin. 

Vanuit daar moest ik de animation iets aanpassen zodat hij de juiste kant op draaide en niet kruislinks langs de andere landen ging. In de code zie dat dat nu niet meer gebeurde. Dit was vooral heel veel proberen, ruim een dag mee geklooid om een goed resultaat te krijgen.


### Malta toevoegen en hover/focus state 

		a.country:focus, a.country:hover{
		background-color: red;
		}

			a.Malta{
		mask-image: url('/SVG/Malta.svg');
		--size-country: 12.5%;
		rotate:  0 1 0 45deg;
		height: calc(var(--size-country) * 1.1);
		--size-translate: calc((var(--size-globe) * .4999) + (var(--size-globe) * .05));
		--translate-animation:  rotateY(0) scale3d(1, 1, .85)  translate3d(120%, 160%, var(--size-translate)) ;
		--translate-animation-end: scale3d(1, 1, .85) translate3d(120%, 160%, var(--size-translate)) ;
		}


Hierbij had ik nu een extra land met een compleet nieuwe startpositie. Dat was even spelen weer, maar was uiteindelijk wel gelukt.



### Week 3: Goede interactie (voor iedereen)
Ik had nu ook een hover en focus state toegevoegd, maar had nog moeite met focus zelf. Een blind persoon kon de applicatie wel gebruiken, omdat die gebruiker de bol toch niet ziet. Maar iemand die alleen toetsenbord gebruikt kan dat niet, hiervoor had ik een aantal ideeën: De gene die ik uiteindelijk onsuccesvol had toegevoegd, was een ul met tabindex, die op de plek van de landen gepositioneerd werd. Zo kon een scrollend persoon en een tabben persoon ook de naam van het land zien. 

			<ul>
		<li id="NL">The Netherlands</li>
		<li id="Germany">Germany</li>
		<li id="Spain">Spain</li>
		<li id="Malta">Malta</li>
		<li id="Estonia">Estonia</li>
	</ul>


		ul:after{
		content: "";
		position: fixed;
		mask: linear-gradient(-180deg,red 45%,transparent 45%, transparent 55%, red 55%);
		height: 100vh;
		width: 15vw;
		
		top: 0;
		left: 0;
		background-color: var(--bg-color);
		scroll-snap-type: y mandatory;
		}
	ul li{
		position: absolute;
		transform: translate(0, -50%);
		scroll-margin: 47.5vh;
		scroll-snap-align: center;
		}
	
	ul li#NL{
		top: 250vh;
		
		}
	ul li#Germany{
		top: 240vh;
		}
	
	ul li#Spain{
		top: 260vh;
		}
	
	ul li#Malta{
		top: 210vh;
		}
	ul li#Estonia{
		top: 365vh
		}


Dit werkte uiteindelijk toch niet, omdat de landen te dicht op elkaar gepositioneerd stonden. Toch jammer dat Europa zo klein was. Dit was dus niet optimaal. Ik vond het niet mooi dat er verschillende landen tegelijk op het beeld stonden. Ook werd daardoor het land op de bol niet geselecteerd, maar de list-item. Niet ideaal. Achteraf had ik hiervoor de :has selector kunnen gebruiken. Maar dat is nu iets wat laat.

Ik had hier ook een scroll-snap in gezet, dat maakte de interactie vervelend en niet soepel, snel weggehaald.


Uiteindelijk had ik ook een disabled state toegevoegd en ben ik doorgegaan met een aantal landen toevoegen, plus een betere tabindex. De landen waren Finland en Frankrijk, dit was nu niet meer zo moeilijk




