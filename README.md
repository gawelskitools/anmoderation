<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <title>Analyse-Tool – Export mit HTML- & Textvorschau</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 960px; margin: auto; padding: 20px; }
    textarea, select, input[type="text"] {
      width: 100%; margin-top: 5px; padding: 6px; font-family: monospace;
    }
    textarea { height: 180px; }
    .analyse-block { margin-bottom: 30px; border-top: 1px solid #ccc; padding-top: 20px; }
    .button-group { margin-top: 10px; display: flex; gap: 10px; flex-wrap: wrap; }
    h2 { margin-top: 40px; }
  </style>
</head>
<body>
  <h1>Analyse-Tool – XML mit HTML- & Klartext-Vorschau</h1>

  <label for="beitragstitel">Beitragstitel:</label>
  <input type="text" id="beitragstitel" value="Sanierung des Großherzoglichen Palais" />

  <label for="slugId">Slug-ID:</label>
  <input type="text" id="slugId" value="sanierung-grossherzogliches-palais-neustrelitz" />

  <label for="ressort">Ressort:</label>
  <select id="ressort">
    <option>Kultur</option>
    <option selected>Gesellschaft</option>
    <option>Panorama</option>
  </select>

  <label for="tags">Tags:</label>
  <input type="text" id="tags" value="Neustrelitz, Sanierung, Denkmal, Hotel, Palais" />

  <div class="analyse-block">
    <h2>Transkript</h2>
    <textarea id="transkript">Der Wohnsitz des letzten Großherzogs von Mecklenburg-Strelitz wird zum Hotel...</textarea>
    <label>Zitate erlaubt?</label>
    <select id="zitateTranskript">
      <option>true</option>
      <option selected>false</option>
    </select>
    <label>Gewichtung:</label>
    <select id="weightTranskript">
      <option>25</option>
      <option selected>75</option>
      <option>100</option>
    </select>
  </div>

  <div class="analyse-block">
    <h2>Anmoderation</h2>
    <textarea id="anmoderation">Das Großherzogliche Palais in Neustrelitz – ist das einzige verbliebene Haus...</textarea>
    <label>Zitate erlaubt?</label>
    <select id="zitateAnmoderation">
      <option>true</option>
      <option selected>false</option>
    </select>
    <label>Gewichtung:</label>
    <select id="weightAnmoderation">
      <option>25</option>
      <option selected>25</option>
      <option>50</option>
    </select>
  </div>

  <h2>Finale XML-Ausgabe</h2>
  <textarea id="finalXml" placeholder="Hier erscheint das finale XML..."></textarea>
  <div class="button-group">
    <button onclick="generateFinalXML()">XML erstellen</button>
    <button onclick="copyToClipboard()">Kopieren</button>
  </div>

<script>
  function escapeXml(str) {
    return str.replace(/&/g, "&amp;")
              .replace(/</g, "&lt;")
              .replace(/>/g, "&gt;")
              .replace(/"/g, "&quot;")
              .replace(/'/g, "&apos;");
  }

  function generateFinalXML() {
    const now = new Date();
    const readable = now.toLocaleDateString("de-DE") + ", " + now.toLocaleTimeString("de-DE");
    const iso = now.toISOString();

    const titel = escapeXml(document.getElementById("beitragstitel").value);
    const slug = escapeXml(document.getElementById("slugId").value);
    const ressort = escapeXml(document.getElementById("ressort").value);
    const tags = escapeXml(document.getElementById("tags").value);
    const transkript = escapeXml(document.getElementById("transkript").value);
    const zitatT = document.getElementById("zitateTranskript").value;
    const weightT = document.getElementById("weightTranskript").value;
    const anmoderation = escapeXml(document.getElementById("anmoderation").value);
    const zitatA = document.getElementById("zitateAnmoderation").value;
    const weightA = document.getElementById("weightAnmoderation").value;

    const klartextVorschau = `
Finale redaktionelle Online-Ausgabe

Überschrift (max. 60 Zeichen)
Historisches Palais in Neustrelitz wird Hotel

Kurzbeschreibung (max. 150 Zeichen)
Nach Jahren des Verfalls wird das Großherzogliche Palais in Neustrelitz privat saniert – denkmalgerecht, detailreich und bald als Hotel eröffnet.

SEO-Keywords (max. 10)
Neustrelitz, Großherzogliches Palais, Sanierung, Denkmal, Hotelumbau, Mecklenburg, historische Gebäude, Restaurierung, Kulturdenkmal, Handwerker

SEO-Themen (max. 5)
Denkmalschutz, Architektur, Regionalgeschichte, Tourismus, Hotelentwicklung`;

    const htmlVorschau = `
<div class="online-ausgabe-vorschau">
  <h2>Historisches Palais in Neustrelitz wird Hotel</h2>
  <p><strong>Kurzbeschreibung:</strong> Nach Jahren des Verfalls wird das Großherzogliche Palais in Neustrelitz privat saniert – denkmalgerecht, detailreich und bald als Hotel eröffnet.</p>
  <div class="seo">
    <h3>SEO-Keywords</h3>
    <ul>
      <li>Neustrelitz</li>
      <li>Großherzogliches Palais</li>
      <li>Sanierung</li>
      <li>Denkmal</li>
      <li>Hotelumbau</li>
      <li>Mecklenburg</li>
      <li>historische Gebäude</li>
      <li>Restaurierung</li>
      <li>Kulturdenkmal</li>
      <li>Handwerker</li>
    </ul>
    <h3>SEO-Themen</h3>
    <ul>
      <li>Denkmalschutz</li>
      <li>Architektur</li>
      <li>Regionalgeschichte</li>
      <li>Tourismus</li>
      <li>Hotelentwicklung</li>
    </ul>
  </div>
</div>`;

    const xml = `<?xml version="1.0" encoding="UTF-8"?>
<online-ausgabe version="1.0">
  <anweisung-fuer-ki>
    <beschreibung>Bitte führe alle Aufgaben vollständig und ohne Rückfragen aus.</beschreibung>
    <aufgaben>
      <aufgabe>
        <ziel>Analyse der Inhalte</ziel>
        <schritte>
          <schritt nummer="2">Zentrale Fakten herausfiltern</schritt>
          <schritt nummer="3">Zentrales Thema formulieren</schritt>
          <schritt nummer="4">Informative Absicht erkennen</schritt>
          <schritt nummer="5">Beteiligte Personen benennen</schritt>
          <schritt nummer="6">Prognose erstellen</schritt>
        </schritte>
        <gilt-fuer>
          <quelle>Transkript</quelle>
          <quelle>Anmoderation</quelle>
        </gilt-fuer>
      </aufgabe>
      <aufgabe>
        <ziel>Erstellung redaktioneller Online-Elemente</ziel>
        <elemente>
          <ueberschrift max-laenge="60" />
          <kurzbeschreibung max-laenge="150" />
          <keywords max="10" />
          <themen max="5" />
        </elemente>
      </aufgabe>
      <aufgabe>
        <ziel>Doppelte Vorschauausgabe</ziel>
        <formate>
          <format typ="text">Mit Zwischenüberschriften</format>
          <format typ="html">Strukturierte Boxenvorschau</format>
        </formate>
      </aufgabe>
    </aufgaben>
    <hinweise>
      <hinweis>Keine Rückfrage stellen</hinweis>
      <hinweis>Keine CDATA verwenden</hinweis>
      <hinweis>Ergebnisse direkt ins XML schreiben</hinweis>
    </hinweise>
  </anweisung-fuer-ki>

  <quelle>
    <id>beitrag-${readable}</id>
    <zeitstempel>${iso}</zeitstempel>
    <slug-id>${slug}</slug-id>
    <art>TV-Beitrag</art>
    <titel>${titel}</titel>
    <themenvorgabe>
      <ressort>${ressort}</ressort>
      <tags>${tags}</tags>
    </themenvorgabe>
    <analysearten>
      <transkript gewichtung="${weightT}">
        <zitate-erlaubt>${zitatT}</zitate-erlaubt>
        <originaltext>${transkript}</originaltext>
        <analyse>
          <schritt nummer="2"></schritt>
          <schritt nummer="3"></schritt>
          <schritt nummer="4"></schritt>
          <schritt nummer="5"></schritt>
          <schritt nummer="6"></schritt>
        </analyse>
      </transkript>
      <anmoderation gewichtung="${weightA}">
        <zitate-erlaubt>${zitatA}</zitate-erlaubt>
        <originaltext>${anmoderation}</originaltext>
        <analyse>
          <schritt nummer="2"></schritt>
          <schritt nummer="3"></schritt>
          <schritt nummer="4"></schritt>
          <schritt nummer="5"></schritt>
          <schritt nummer="6"></schritt>
        </analyse>
      </anmoderation>
    </analysearten>
  </quelle>
  <editorial-metadata>
    <ueberschrift länge-max="60"></ueberschrift>
    <kurzbeschreibung länge-max="150"></kurzbeschreibung>
    <seo>
      <keywords max="10">
        <keyword></keyword>
      </keywords>
      <themen max="5">
        <thema></thema>
      </themen>
    </seo>
  </editorial-metadata>
  <vorschau>
    <text><![CDATA[${klartextVorschau}]]></text>
    <html><![CDATA[${htmlVorschau}]]></html>
  </vorschau>
</online-ausgabe>`;

    document.getElementById("finalXml").value = xml;
  }

  function copyToClipboard() {
    const out = document.getElementById("finalXml");
    out.select();
    document.execCommand("copy");
  }
</script>
</body>
</html>