---
layout: post 
title: Sphinx-4 Naudojamas
lang: lt
categories:
    - lt
summary: Sphinx-4 panaudojimo pavyzdys(Java kalba)     
date: 2013-09-22 23:59    
---


Sphinx-4
---------------------
[Sphinx-4][url-Sphinx4-oficial] šnekos atpažintuvas yra parašytas naudojantis tik [Java][url-Sphinx4-java] programavimo kalbą. Atapžintuvo versija buvo kuriame Sphinx grupės kurią sudarė Carnegie Mellon University, Sun Microsystems Laboratories, Mitsubishi Electric Research Labs (MERL) ir Hewlett Packard. Prie darbų prisidėjo University of California at Santa Cruz (UCSC) ir  Massachusetts Institute of Technology (MIT).

Sphinx-4 versija buvo pradėta perašant Sphinx-3 į kitą programavimo kalbą. Tačiau vykdant transformaciją buvo įgyvendinti atpažinimo architektūrinio dizaino pakeitimai, kurie leidžia teigti kad naujoji versija yra tinkamesnė tyrimas. 

### Savybės

*	Realaus laiko ir išsaugotų signalų atpažinimas
*	Atpažinimas pavieniui tariamų ir ištisinės šnekos
*	Lengvai keičiami sistemos modulių realizacijos:
	*	[Šnekos nustatymo](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/package-summary.html)
	*	[Preemphasizer](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/filter/Preemphasizer.html)
	*	[Langų funkcija](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/window/RaisedCosineWindower.html)
	*	[Greitoji Furjė tranformacija](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/transform/DiscreteFourierTransform.html)
	* 	[Melų dažnių coeficientų skaičiavimas](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/frequencywarp/MelFrequencyFilterBank.html)
	*	[Discrečioji kosinuso transformacija](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/transform/DiscreteCosineTransform.html)
	*	[Kepsrtrinis vidurkinimo normalizavimas](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/feature/BatchCMN.html)
	*	[Požymių gavyba](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/frontend/feature/DeltasFeatureExtractor.html)
*	Programiškai abstraktus [akustinis modelis](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/linguist/acoustic/package-summary.html), kuris palaiko ir Sphinx-3 akustinį modelį
*	Programiškai abstraktus paieškos [algoritmas grafe](http://cmusphinx.sourceforge.net/sphinx4/javadoc/edu/cmu/sphinx/decoder/search/package-summary.html)


Aplinkos ir bibliotekų pasiruošimas
----------------------

### Reikalinga programinė įranga

Sphinx-4 veikimas buvo tikrinamas Solaris, Mac OS X, Linux and MS Windows. Sphinx-4 surinkimas ir paleidimas reikalauja papildomos įrangos:

*   Orcale(Sun) Java - Java virtuali mašina privaloma surinkimui ir paleidimui. [Parsisiųsti][url-java-download]. Norit kompiliuoti ir surinkti Sphinx-4 reikia parsiųsti: "Java Platform (JDK)". Rašymo metu buvo tikrinta "Java SE 7u40".
*   Maven - komandinės eilutės įrankis reikalingas mavenizuotos Sphinx-4 surinkimui [Parsisiųsti][url-maven-download]. Rašymo metu buvo sėkmingai naudojama 3.1.0 versija.
*   Git - išeities kodo apsikeitimo programa _pasirinktinai_ gali reikėti jei norėsite keisti kodą ir jį atgal siųst serverį.

### Demonstracijos paleidimas
* Skaitykite daugiau techninės informacijos [Mondhs@Github/cmusphinx-mavenized-sample][url-sample-project]
* Parsiųskite demonstracinį kodą: [cmusphinx-mavenized-sample][url-sample-download]. 
* išskleiskite master.zip į `c:\cmushinx\sphinx4\pvz`. 
* Komandinėje eilutėje direktorijoje `c:\cmushinx\sphinx4\pvz\cmusphinx-mavenized-sample-master\sphinx4-hello_world-lt` sukompiliuokite kodą `mvn package`. 
	*	Pirmą kartą maven programinė įrangą gali ilgai užtrukti gana daug parsisiųsti bibliotekų iš interneto. Sekantys leidimai 
	*   Leidžiant antrą kartą turėsite matyti kažką panašaus

```
	4231 [INFO] Building jar: /home/as/src/speech/cmusphinx-mavenized-sample/sphinx4-hello_world-lt/target/sphinx4-hello_world-lt-1.0-SNAPSHOT.jar
	4258 [INFO] ------------------------------------------------------------------------
	4258 [INFO] BUILD SUCCESS
	4258 [INFO] ------------------------------------------------------------------------
	4260 [INFO] Total time: 2.858s
	4260 [INFO] Finished at: Sun Sep 22 20:57:37 EEST 2013
	4476 [INFO] Final Memory: 25M/982M
	4478 [INFO] ------------------------------------------------------------------------
```	

* Paleiskite pavyzdį: `mvn exec:java -Ddemo`. Jei viskas gerai turėsite matyti kažką panašaus:

```
	mvn exec:java -Ddemo
	1697 [INFO] Scanning for projects...
	1862 [INFO]                                                                         
	1862 [INFO] ------------------------------------------------------------------------
	1863 [INFO] Building Shinx - Demo - hello_world module(Lithuanian) 1.0-SNAPSHOT
	1863 [INFO] ------------------------------------------------------------------------
	1875 [INFO] 
	1875 [INFO] >>> exec-maven-plugin:1.1:java (default-cli) @ sphinx4-hello_world-lt >>>
	1879 [INFO] 
	1879 [INFO] <<< exec-maven-plugin:1.1:java (default-cli) @ sphinx4-hello_world-lt <<<
	1937 [INFO] 
	1937 [INFO] --- exec-maven-plugin:1.1:java (default-cli) @ sphinx4-hello_world-lt ---
	Sakyk: (EIK | VARYK ) [ ( VIENĄ | DU | TRIS | KETURIS | PENKIS ) METRUS ] (PIRMYN | ATGAL) arba (SUK | GRĘŽKIS ) ( KAIRĖN | DEŠINĖN )
	Pradėk šnekėti. spausk Ctrl-C nutraukti.

	tu sakei: eik pirmyn

	Pradėk šnekėti. spausk Ctrl-C nutraukti.

	tu sakei: suk kairėn

	Pradėk šnekėti. spausk Ctrl-C nutraukti.

	^C
```  

### Pavyzdžio programos kodas

* Kodas [RobotoValdymas.java](https://github.com/mondhs/cmusphinx-mavenized-sample/blob/master/sphinx4-hello_world-lt/src/main/java/RobotoValdymas.java) ir atpažintuva konfigūracija [lt_robotas.config.xml](https://github.com/mondhs/cmusphinx-mavenized-sample/blob/master/sphinx4-hello_world-lt/src/main/resources/lt_robotas.config.xml):

```

	import edu.cmu.sphinx.frontend.util.Microphone;
	import edu.cmu.sphinx.recognizer.Recognizer;
	import edu.cmu.sphinx.result.Result;
	import edu.cmu.sphinx.util.props.ConfigurationManager;


	/**
	 * Pavyzdinė klasė
	 */
	public class RobotoValdymas{
		/**
		 * Pagrindinis metodas kiečiamas OS.
		 */
		public static void main(String[] args) {
			//konfigūracijos tvarkytojas
			ConfigurationManager konfiguracija = new ConfigurationManager(RobotoValdymas.class.getResource("lt_robotas.config.xml"));
			
			Recognizer atapzintuvas = (Recognizer) konfiguracija.lookup("recognizer");
			atapzintuvas.allocate();

			// pradėti įrašą per mikrofoną
			Microphone mikrofonas = (Microphone) konfiguracija.lookup("microphone");
			mikrofonas.startRecording();

			System.out.println("Sakyk: (EIK | VARYK ) [ ( VIENĄ | DU | TRIS | KETURIS | PENKIS ) METRUS ] (PIRMYN | ATGAL) arba (SUK | GRĘŽKIS ) ( KAIRĖN | DEŠINĖN )");

			// Sukti amžiną ciklą atpažinimui
			while (true) {
				System.out.println("Pradėk šnekėti. spausk Ctrl-C nutraukti.\n");

				Result rezultatas = atapzintuvas.recognize();

				if (rezultatas != null) {
					String rezultatoTekstas = rezultatas.getBestFinalResultNoFiller();
					System.out.println("tu sakei: " + rezultatoTekstas + '\n');
				} 
			}
		}
	}

```

[url-Sphinx4-java]: http://www.java.com/ 	"Java Official"
[url-Sphinx4-oficial]: http://cmusphinx.sourceforge.net/sphinx4/ "Sphinx-4 official"
[url-java-download]: http://www.oracle.com/technetwork/java/javase/downloads/index.html 	"Java Download"
[url-maven-download]: http://maven.apache.org/download.cgi 	"Maven Download"
[url-sample-download]: https://github.com/mondhs/cmusphinx-mavenized-sample/archive/master.zip	"CMU Sphinx Mavenized Samples Download"
[url-sample-project]: https://github.com/mondhs/cmusphinx-mavenized-sample "CMU Sphinx Mavenized Samples project"
