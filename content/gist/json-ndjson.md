---
title: "Convert json to nd-json for elasticsearch"
date: 2024-03-29T20:29:51+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["elasticsearch","json","ndjson"]
slug: "json-to-ndjson-elasticsearch"
categories: ["Elastic"]
---


Convert JSON to nd-json for bulk indexing into Elasticsearch.



### movies.json

```json
[
  {
    "title_x": "Student of the Year 2",
    "imdb_id": "tt7255568",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/3/3c/Student_of_the_year_2_Poster.jpg/220px-Student_of_the_year_2_Poster.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Student_of_the_Year_2",
    "title_y": "Student of the Year 2",
    "original_title": "Student of the Year 2",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "146",
    "genres": "Drama|Romance|Sport",
    "imdb_rating": 2.5,
    "imdb_votes": 12671,
    "story": "A student must face off against bullies and overcome hurdles  both academic and romantic  to win his college's coveted Student of the Year trophy.",
    "summary": "A student must face off against bullies and overcome hurdles  both academic and romantic  to win his college's coveted Student of the Year trophy.",
    "tagline": "",
    "actors": "Tiger Shroff|Ananya Panday|Tara Sutaria|Aditya Seal|Manoj Pahwa|Ayesha Raza|Rajesh Kumar|Manasi Joshi Roy|Samir Soni|Gul Panag|Manjot Singh|Sahil Anand|Harsh Beniwal|Daljeet Singh Gujral|",
    "wins_nominations": "",
    "release_date": "10 May 2019 (USA)"
  },
  {
    "title_x": "De De Pyaar De",
    "imdb_id": "tt8647400",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/c/c4/De_De_Pyaar_De_Poster.jpg/220px-De_De_Pyaar_De_Poster.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/De_De_Pyaar_De",
    "title_y": "De De Pyaar De",
    "original_title": "De De Pyaar De",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "135",
    "genres": "Comedy|Romance",
    "imdb_rating": 6.6,
    "imdb_votes": 5387,
    "story": "a 50 year old Aashish living London away from his family fro many years He falls in love with a 26 year Ayesha.Thou officially not divorced from his first wife Manju.Aashish wants her permission to get married to Ayesha.Upon reaching his house in India  Ayesha finds that Aashish kids are of the same age as her.Aashish introduces Ayesha as his secretary to his family he also finds that his daughter is about to get married and VK whose become close family friend is trying to woo her Manju.",
    "summary": "A 50-year-old single father faces disapproval from his family and his ex-wife when he falls in love with a 26-year-old woman.",
    "tagline": "",
    "actors": "Ajay Devgn|Tabu|Rakul Preet Singh|Jimmy Sheirgill|Alok Nath|Madhumalti Kapoor|Harry Anton|Gergo Baranyi|Sammy John Heaney|Bhavin Bhanushali|Gandharv Dewan|Gulnaaz Khan|Kumud Mishra|Sunny Singh Nijjar|",
    "wins_nominations": "",
    "release_date": "17 May 2019 (USA)"
  },
  {
    "title_x": "India's Most Wanted (film)",
    "imdb_id": "tt8484942",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/a/ae/India%27s_Most_Wanted_poster.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/India%27s_Most_Wanted_(film)",
    "title_y": "India's Most Wanted",
    "original_title": "India's Most Wanted",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "123",
    "genres": "Action|Thriller",
    "imdb_rating": 4.4,
    "imdb_votes": 1449,
    "story": "India's Most Wanted is a Bollywood action thriller film directed by Raj Kumar Gupta and starring Arjun Kapoor. The film is about tracking a terrorist in a secret mission and arresting him without firing bullets. It pays tribute to unsung heroes of our society.",
    "summary": "A group of intelligence officers embark on a top secret mission to track down a wanted international criminal.",
    "tagline": "",
    "actors": "Arjun Kapoor|Sudev Nair|Rajesh Sharma|Prasanth|Santilal Mukherjee|",
    "wins_nominations": "",
    "release_date": "24 May 2019 (USA)"
  },
  {
    "title_x": "Yeh Hai India",
    "imdb_id": "tt5525846",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/d/db/Yeh_Hai_India_Official_Poster.jpg/220px-Yeh_Hai_India_Official_Poster.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Yeh_Hai_India",
    "title_y": "Yeh Hai India",
    "original_title": "Yeh Hai India",
    "is_adult": 0,
    "year_of_release": "2017",
    "runtime": "128",
    "genres": "Action|Adventure|Drama",
    "imdb_rating": 5.7,
    "imdb_votes": 169,
    "story": "Yeh Hai India  follows the story of a 25 years old NRI  who is born and brought up in U.K and shares the same stereotype views of India  which is known for its vast population  pollution and poverty. However protagonist finds new development in media or probably an 'other side of same coin' of India  which is also known for successful mars mission in first attempt  a nation which proudly holds title 'God of Cricket' for Sachin Tendulkar  who is again an Indian and a nation which is known for its holy generosity with icons like Mother Teresa\".",
    "summary": "Yeh Hai India  follows the story of a 25 years old NRI  who is born and brought up in UK. He shares the same stereotypical views about India that most NRIs have and how his perception changes.",
    "tagline": "A Film for Every Indian",
    "actors": "Gavie Chahal|Mohan Agashe|Mohan Joshi|Lom Harsh|",
    "wins_nominations": "2 wins & 1 nomination",
    "release_date": "24 May 2019 (India)"
  },
  {
    "title_x": "Kabir Singh",
    "imdb_id": "tt8983202",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/d/dc/Kabir_Singh.jpg/220px-Kabir_Singh.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Kabir_Singh",
    "title_y": "Kabir Singh",
    "original_title": "Kabir Singh",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "173",
    "genres": "Drama|Romance",
    "imdb_rating": 7.2,
    "imdb_votes": 20535,
    "story": "This Sandeep Vanga directorial is a remake of the 2017 Telugu movie Arjun Reddy. The plot revolves around an alcoholic surgeon battling temper issues. Things get worse as he watches the love of his life marry another man. High on emotions coupled with impressive action the film promises an intense watch.",
    "summary": "Kabir Singh is a remake of a Telugu movie Arjun Reddy (2017)  where a short-tempered house surgeon gets used to drugs and drinks when his girlfriend is forced to marry another person.",
    "tagline": "",
    "actors": "Shahid Kapoor|Kiara Advani|Soham Majumdar|Arjan Bajwa|Suresh Oberoi|Kamini Kaushal|Adil Hussain|Nikita Dutta|Amit Sharma|Anurag Arora|Dolly Mattoo|Aanchal Chauhan|Suparna Marwah|Swati Seth|",
    "wins_nominations": "",
    "release_date": "20 June 2019 (USA)"
  },
  {
    "title_x": "Article 15 (film)",
    "imdb_id": "tt10324144",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/1/11/Article_15_Poster.jpg/220px-Article_15_Poster.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Article_15_(film)",
    "title_y": "Article 15",
    "original_title": "Article 15",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "130",
    "genres": "Crime|Drama",
    "imdb_rating": 8.3,
    "imdb_votes": 13417,
    "story": "In the rural heartlands of India  an upright police officer sets out on a crusade against violent caste-based crimes and discrimination.",
    "summary": "In the rural heartlands of India  an upright police officer sets out on a crusade against violent caste-based crimes and discrimination.",
    "tagline": "Farq Bahut Kar Liya| Ab Farq Laayenge.",
    "actors": "Ayushmann Khurrana|Nassar|Manoj Pahwa|Kumud Mishra|Isha Talwar|Sayani Gupta|Mohammed Zeeshan Ayyub|Subhrajyoti Barat|Sushil Pandey|Aakash Dabhade|Ashish Verma|Ronjini Chakraborty|Veen|Sumbul Touqueer|",
    "wins_nominations": "1 win",
    "release_date": "28 June 2019 (USA)"
  },
  {
    "title_x": "One Day: Justice Delivered",
    "imdb_id": "tt8130558",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/f/f7/One_Day-_Justice_Delivered.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/One_Day:_Justice_Delivered",
    "title_y": "One Day: Justice Delivered",
    "original_title": "One Day: Justice Delivered",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "124",
    "genres": "Action|Crime|Thriller",
    "imdb_rating": 6.8,
    "imdb_votes": 590,
    "story": "A Crime Branch Special Officer Investigates the Serial disappearance of high profile individuals in a state capital.",
    "summary": "A Crime Branch Special Officer Investigates the Serial disappearance of high profile individuals in a state capital.",
    "tagline": "",
    "actors": "Anupam Kher|Kumud Mishra|Esha Gupta|Zakir Hussain|Rajesh Sharma|Murli Sharma|Deepshika Nagpal|",
    "wins_nominations": "",
    "release_date": "5 July 2019 (India)"
  },
  {
    "title_x": "Super 30 (film)",
    "imdb_id": "tt7485048",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/2/29/Super_30_The_Film.jpg/220px-Super_30_The_Film.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Super_30_(film)",
    "title_y": "Super 30",
    "original_title": "Super 30",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "154",
    "genres": "Biography|Drama",
    "imdb_rating": 8.2,
    "imdb_votes": 13972,
    "story": "Anand Kumar  a Mathematics genius from a modest family in Bihar who is made to believe that only a King's son can become a king is on a mission to prove that even the poor man can create some of the world's most genius minds. He starts a training program named 'Super 30' to help 30 IIT aspirants crack the entrance test and make them highly successful professionals.",
    "summary": "Based on life of Patna-based mathematician Anand Kumar who runs the famed Super 30 program for IIT aspirants in Patna.",
    "tagline": "Inspired by the Life of Anand Kumar & His Students",
    "actors": "Hrithik Roshan|Mrunal Thakur|Nandish Singh|Virendra Saxena|Sadhana Singh|Aditya Srivastava|Sanket Deshpande|Pankaj Tripathi|Vaibhav Gupta|Ali Haji|Rajesh Sharma|Deepali Kumar|Chittaranjan Giri|Ganesh Kumar|",
    "wins_nominations": "",
    "release_date": "12 July 2019 (USA)"
  },
  {
    "title_x": "Batla House",
    "imdb_id": "tt8869978",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/e/ed/Batla_House_poster.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Batla_House",
    "title_y": "Batla House",
    "original_title": "Batla House",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "146",
    "genres": "Action|Drama|Thriller",
    "imdb_rating": 7.3,
    "imdb_votes": 5556,
    "story": "This action thriller is based on the real-life incident of 'Batla House Encounter'  officially known as Operation Batla House  to the silver screen. The incident took place on September 19  2008  against Indian Mujahideen (IM) terrorists in Batla House locality in Jamia Nagar  Delhi.",
    "summary": "After a deadly encounter  a police officer works to prove that the police acted lawfully.",
    "tagline": "The Story of India's Most Decorated / Controversial Cop",
    "actors": "John Abraham|Nora Fatehi|Mrunal Thakur|Rajesh Sharma|Ravi Kishan|Sonam Arora|Sahidur Rahaman|Manish Chaudhary|Anil Rastogi|Kranti Prakash Jha|Amruta Sant|Anil Khopkar|Aditi Gulati|Jitendra Trehan|",
    "wins_nominations": "",
    "release_date": "15 August 2019 (USA)"
  },
  {
    "title_x": "Mission Mangal",
    "imdb_id": "tt9248972",
    "poster_path": "https://upload.wikimedia.org/wikipedia/en/thumb/b/b7/Mission_Mangal.jpg/220px-Mission_Mangal.jpg",
    "wiki_link": "https://en.wikipedia.org/wiki/Mission_Mangal",
    "title_y": "Mission Mangal",
    "original_title": "Mission Mangal",
    "is_adult": 0,
    "year_of_release": "2019",
    "runtime": "130",
    "genres": "Drama|History",
    "imdb_rating": 6.5,
    "imdb_votes": 7165,
    "story": "Based on true events of the Indian Space Research Organisation (ISRO) successfully launching the Mars Orbiter Mission (Mangalyaan)  making it the least expensive mission to Mars.",
    "summary": "Based on true events of the Indian Space Research Organisation (ISRO) successfully launching the Mars Orbiter Mission (Mangalyaan)  making it the least expensive mission to Mars.",
    "tagline": "This Independence Day...The Sky is Not the Limit",
    "actors": "Akshay Kumar|Vidya Balan|Nithya Menon|Taapsee Pannu|Sonakshi Sinha|Kirti Kulhari|H.G. Dattatreya|Sharman Joshi|Sanjay Kapoor|Dalip Tahil|Vikram Gokhale|Mohammed Zeeshan Ayyub|Purab Kohli|",
    "wins_nominations": "",
    "release_date": "15 August 2019 (USA)"
  }
]
```

Install `jq` on your linux machine and hit below command

```sh
cat movies.json | \
jq -c '.[] | {"index": {"_index": "movies", "_id": .id}}, .' > nd-movies.json
```

Verify newly created `nd-movies.json` file.
