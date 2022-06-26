https://github.blog/changelog/2021-08-25-github-actions-reduce-duplication-with-action-composition/

# Rust

let a = [1, 2, 3];
for i in a {
    println!("{}", i);
}
for i in a {            // doesn't work
    println!("{}", i);
}

let m = [[0u8; 4]; 6];
println!("{:?}", m);
for r in 0..m.len() {
    for e in 0..m[r].len() {
        println!("{}", m[r][e]);
    }
}
