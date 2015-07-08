# gitTest

router.get('/csv', function(req, res, next) {
    var stringfier = csv.stringify({
        rowDelimiter: 'windows',
        columns: ['age', 'name'],
        header: true
    });
    var parser = csv.parse();
    var transformer = csv.transform(function(data) {
        console.log(data[0]);
        return {
            age: data[0],
            name: data[1]
        };
    });
    var path = join(__dirname, '/test.csv');
    var readStream = fs.createReadStream(path);

    res.attachment('test.csv');
    readStream.pipe(parser).pipe(transformer).pipe(stringfier).pipe(res);
});
