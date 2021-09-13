```sql
insert into Genre(name) values
('pop'),
('rock'),
('jazz'),
('hiphop'),
('indie'),
('blues');

insert into Singer(name) values
('AC/DC'),
('Bon Jovi'),
('Moby'),
('Morgue Vanguard'),
('Da Molotov'),
('Океан Эльзы'),
('Eminem');

insert into SingerGenre(singer_id, genre_id) values
(1, 1), (1, 2), (2, 2), (3, 2), (4, 2), (5, 1), (5, 2), (6, 1), (6, 4);

insert into Album(name, pub_year) values
('Long Ambients 2', 2019), ('Demi Masa', 2018), ('Everything Was Beau', 2018),
('Kamikaze', 2018), ('Con Todo Respeto', 2004), ('Songs of Faith', 1993),
('St. Anger', 2003), ('Back in Black', 1998), ('New Jersey', 1988);

insert into Track(name, duration, album_id) values
('LA12', 2820, 1), ('LA13', 1620, 1), ('LA14', 2340, 1), ('LA15', 1920, 1), ('LA16', 1800, 1),
('LA17', 2520, 1), ('Junta Titimangsa', 139, 2), ('Breakadawn', 275, 2), ('CSDB FM feat. Iwa K', 351, 2),
('Testamen', 297, 2), ('Buckshot Funk', 137, 2), ('Rotasi Baja', 58, 2),
('Di Hadapan Babylon', 266, 2);

insert into Collection(name, pub_year) values
('The White Stripes', 2020), ('RAM Drum &', 2020),
('Neil Young Archives', 2020), ('Kate Bush Remas', 2018),
('John Maus', 2018), ('Civilisation', 2021),
('Limp Pumpo Full', 2021), ('David Bowie A', 2017);

insert into SingerAlbum(singer_id, album_id) values
(5, 1), (6, 2), (5, 3), (7, 4), (7, 5);

insert into TrackCollection(track_id, collection_id) values
(1, 1), (2, 1), (3, 1), (4, 1), (5, 1), (6, 1);

#количество исполнителей в каждом жанре;
select Genre.name, count(Singer.id) as singer_amount from Genre
join SingerGenre on Genre.id = SingerGenre.genre_id
join Singer on SingerGenre.singer_id = Singer.id
group by Genre.name;

#количество треков, вошедших в альбомы 2019-2020 годов;
select count(Track.id) as track_amount from Track
join Album on Track.album_id = Album.id
where Album.pub_year between 2019 and 2020;

#средняя продолжительность треков по каждому альбому;
select Album.name, avg(Track.duration) as track_duration from Album
join Track on Track.album_id = Album.id
group by Album.id;

#все исполнители, которые не выпустили альбомы в 2020 году;
select Singer.name, Album.pub_year from Singer
join SingerAlbum on SingerAlbum.singer_id = Singer.id
join Album on SingerAlbum.album_id = Album.id
where pub_year <> 2020;

#названия сборников, в которых присутствует конкретный исполнитель (выберите сами); 
!!!Не совсем понял, что именно нужно было сделать в этом задании!!!
select name from Collection
where name like '%Kate%';

#название альбомов, в которых присутствуют исполнители более 1 жанра;
select Album.name from Album
join SingerAlbum on Album.id = SingerAlbum.album_id
join Singer on Singer.id = SingerAlbum.singer_id
join SingerGenre on Singer.id = SingerGenre.singer_id
join Genre on Genre.id = SingerGenre.genre_id
group by Album.name
having count(Genre.id) > 1;

#наименование треков, которые не входят в сборники;
select Track.name from Track
left join TrackCollection on Track.id = TrackCollection.track_id
left join Collection on Collection.id = TrackCollection.collection_id
where TrackCollection.collection_id is null;

#исполнителя(-ей), написавшего самый короткий по продолжительности трек (теоретически таких треков может быть несколько);
select Singer.name, Track.duration from Singer
join SingerAlbum on Singer.id = SingerAlbum.singer_id
join Album on Album.id = SingerAlbum.album_id
join Track on Track.album_id = Album.id
where Track.duration = (select min(Track.duration) from Track) LIMIT 1;

#название альбомов, содержащих наименьшее количество треков.
select Album.name, count(Track.id) as tracks_amount from Album
join Track on Album.id = Track.album_id
group by Album.name
order by count(Track.id) limit 1;
```
