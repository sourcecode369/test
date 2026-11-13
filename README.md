import pyarrow as pa

def write_for_dai(X, path):
    """Plain parquet that older readers take: text as plain strings, whole numbers as int32,
    no pandas information in the file, format version 1.0."""
    for c in X:
        if X[c].dtype == "string":
            X[c] = X[c].astype(object)
        elif pd.api.types.is_integer_dtype(X[c]):
            X[c] = X[c].astype("int32")
    t = pa.Table.from_pandas(X, preserve_index=False).replace_schema_metadata(None)
    pq.write_table(t, path, version="1.0", compression="snappy", row_group_size=100_000)

out_train, out_test = C.EXP_DIR / "upsell_dai_train.parquet", C.EXP_DIR / "upsell_dai_test.parquet"
out_sample = C.EXP_DIR / "upsell_dai_sample.parquet"     # 5,000 training rows: a quick test that DAI reads the format
write_for_dai(Xtr, out_train)
write_for_dai(Xte, out_test)
write_for_dai(Xtr.head(5000).copy(), out_sample)
table.to_csv(C.EXP_DIR / "dai_columns_upsell.csv", index=False)
for p in (out_train, out_test, out_sample):
    print(f"{p}  {p.stat().st_size / 1e9:.2f} GB")
print("column list with sources:", C.EXP_DIR / "dai_columns_upsell.csv")
