import ast
import pandas as pd
path="llm_pred_output_19NOV24_mismatch_checked.csv"
data=pd.read_csv(path)
column1="input"
column2="model_prediction"

def extract_line_items(predictions):
    try:
        parsed_predictions = ast.literal_eval(predictions)
        return [item['line_item'] for item in parsed_predictions if 'line_item' in item]
    except (ValueError, SyntaxError):
        return []
def find_missing_elements(input_text, line_items_list):
    try:
        parsed_input = ast.literal_eval(input_text) if isinstance(input_text, str) else input_text
        return list(set(parsed_input) - set(line_items_list))
    except (ValueError, SyntaxError):
        return []
data['line_items_list'] = data['model_prediction'].apply(extract_line_items)
data['missing_elements'] = data.apply(lambda row: find_missing_elements(row['input'], row['line_items_list']), axis=1)
data['missing_count'] = data['missing_elements'].apply(len)
#print(data[['input', 'line_items_list', 'missing_elements']].head())
final_output = data[['doc_id', 'doc_name', 'language_code', 'fin_type', 'input',
                     'model_prediction', 'missing_count','line_items_list']]

output_file_path = 'llm-output.csv'
data.to_csv(output_file_path, index=False)
print(f"file saved as: {output_file_path}")
