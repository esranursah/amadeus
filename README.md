# amadeus
case study
import requests
import json

def test_api(url):
    # HTTP status code kontrolü
    response = requests.get(url)
    assert response.status_code == 200, f"Error: Expected 200, but got {response.status_code}"

    # Response içeriği kontrolü
    try:
        data = response.json()
    except json.JSONDecodeError:
        assert False, "Error: Response is not in JSON format"

    assert "data" in data and isinstance(data["data"], list), "Error: Invalid response format"
    
    for flight in data["data"]:
        assert all(key in flight and type(flight[key]) == expected_types[key] for key in expected_types), "Error: Invalid flight structure"

    # Header kontrolü
    content_type_header = response.headers.get("Content-Type", "")
    assert content_type_header.lower() == "application/json", f"Error: Expected 'application/json', but got '{content_type_header}'"

    print("Testler başarıyla tamamlandı.")
    
    return data

# Beklenen veri yapısı
expected_types = {
    "id": int,
    "from": str,
    "to": str,
    "date": str
}

# API URL
api_url = "https://flights-api.buraky.workers.dev/"

# Test fonksiyonunu çağır
data = test_api(api_url)
print("Örnek data: ", data["data"][0])
