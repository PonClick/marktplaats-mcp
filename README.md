# Marktplaats MCP Server

Een MCP (Model Context Protocol) server voor het zoeken en bekijken van advertenties op Marktplaats.nl. Hiermee kan je AI-assistenten zoals Claude toegang geven tot Marktplaats.

## Features

- **Zoeken** - Zoek advertenties met uitgebreide filters (prijs, afstand, conditie, categorie, etc.)
- **Categorie filters** - Filter op categorie-specifieke attributen zoals RAM, schermgrootte, merk, framehoogte, etc.
- **Advertentie details** - Bekijk volledige beschrijvingen, alle afbeeldingen en kenmerken
- **Verkoper info** - Bekijk verkoper beoordelingen en verificatiestatus
- **Categorieën** - Blader door alle Marktplaats categorieën

## Installatie

Je kan op 2 manieren installeren: via `uvx` of handmatig.

### `uvx`

```
claude mcp add --transport stdio marktplaats -- uvx --with 'mcp<2' git+https://github.com/PonClick/marktplaats-mcp marktplaats-mcp
```

of voeg toe in `.claude.json`:

```
"mcpServers": {
  "marktplaats": {
    "type": "stdio",
    "command": "uvx",
    "args": [
      "git+https://github.com/PonClick/marktplaats-mcp",
      "marktplaats-mcp"
    ],
    "env": {}
  }
}
```


### Handmatige installatie

1. Git clone en pip install.
  ```bash
  # Clone de repository
  git clone https://github.com/PonClick/marktplaats-mcp.git
  cd marktplaats-mcp
  
  # Installeer met pip
  pip install -e .
  ```

2. Voeg toe aan je Claude Desktop configuratie (`~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "marktplaats": {
      "command": "marktplaats-mcp"
    }
  }
}
```

## Gebruik

### Beschikbare Tools

#### `search_listings`
Zoek advertenties met filters:
- `query` - Zoekterm
- `category` / `subcategory` - Categorie filter
- `zip_code` - Postcode voor afstand
- `distance_km` - Maximum afstand
- `price_from` / `price_to` - Prijsrange
- `condition` - Conditie (new, used, as_good_as_new, etc.)
- `attribute_ids` - Categorie-specifieke filters (gebruik `get_category_filters`)

#### `get_listing_details`
Haal volledige advertentie details op inclusief:
- Volledige beschrijving
- Alle afbeeldingen
- Kenmerken
- Statistieken (views, bewaard)

#### `get_seller_info`
Verkoper informatie:
- Naam en verificatiestatus
- Gemiddelde beoordeling
- Aantal reviews

#### `list_categories`
Bekijk alle beschikbare categorieën.

#### `get_category_filters`
Haal beschikbare filters op voor een categorie, bijvoorbeeld:
- Laptops: RAM, schermgrootte, processor, SSD/HDD
- Fietsen: Merk, framehoogte, elektrisch

## Voorbeelden

### Zoek een laptop met 16GB RAM
```
search_listings(
    subcategory="laptops",
    price_to=500,
    attribute_ids=[12103]  # 16GB RAM
)
```

### Zoek elektrische Gazelle fietsen
```
search_listings(
    query="gazelle",
    subcategory="elektrische fietsen",
    condition="used"
)
```

## Credits

- **Basis library**: [marktplaats-py](https://github.com/jensjeflensje/marktplaats-py) door [@jensjeflensje](https://github.com/jensjeflensje) - Bedankt voor het bouwen van de uitstekende Python library voor Marktplaats!

## Open Source

Dit project is volledig open source en aangeboden door **lessClick AI**.

## Licentie

MIT License - Zie [LICENSE](LICENSE) voor details.

## Disclaimer

Dit is een onofficieel project en is niet geassocieerd met Marktplaats.nl. Gebruik op eigen risico en respecteer de gebruiksvoorwaarden van Marktplaats.
