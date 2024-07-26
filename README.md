import re
import matplotlib.pyplot as plt
from collections import Counter
import pandas as pd
from wordcloud import WordCloud

# Teks langsung diinputkan
text = """
Long long time ago, in England in Sherwood Forest lived Robin Hood. 
When he was a boy, he had been cheated by a few noblemen. 
Since then he had decided that he would rob the rich and give what he got to the poor.
The Sheriff of Nottingham had made an advertisement that he would give many rewards for the capture of Robin Hood, nobody had ever caught him. 
It was because Robin Hood had a number of friends who served him. They acted as informers. 
When the Sheriff had any plan to catch him, they would warn Robin Hood.
Many rich people were scared of going through Sherwood Forest because they knew that Robin Hood would attack them. 
The Sheriff couldn’t stand it anymore. 
Then he went to ask for the king’s help. 
However, the king refused to send any of his men to help in the capture of Robin Hood.
One day The Sheriff and the noblemen held a competition to choose the best shooter in Nottingham. 
It was for capturing Robin Hood. 
Robin Hood was an excellent shooter. 
Therefore, Robin Hood would participate in the competition to prove that he was the best. 
He had been warned by his servant, but Robin wasn’t willing to listen.
The competition began. 
William, The Sheriff man, and the man in green were trying for the first prize. 
it was time for the last arrow to be shot. 
The winner of this round would be declared the best shooter in Nottingham. 
William could shot very close to the center. 
Then the man in green’s turn made the crowd cheer hysterically. 
His arrow went through William’s arrows and the center of the target. 
Then he shot two more arrows towards the chair on which the Sheriff sat. 
No doubt that the man in green was Robin Hood. 
Immediately Robin Hood pulled off his black wig and then jumped over a wall onto his waiting horse and was gone. 
The Sheriff shouted to his men to catch him, but it was too late. 
Robin Hood escaped successfully.
"""

# Stopwords manual (dapat disesuaikan sesuai kebutuhan)
stop_words = set([
    'the', 'is', 'in', 'and', 'to', 'of', 'a', 'it', 'was', 'he', 'had', 'been', 'that', 'for', 'on', 'with', 'this', 
    'by', 'an', 'as', 'at', 'from', 'or', 'be', 'but', 'they', 'not', 'had', 'any', 'him', 'who', 'her', 'his', 'she', 
    'were', 'have', 'has', 'you', 'we', 'your', 'which', 'there', 'more', 'some', 'such', 'can', 'than', 'up', 'over', 
    'all', 'any', 'our', 'if', 'so', 'has', 'just', 'about', 'into', 'after', 'when', 'while', 'where', 'after', 'will', 
    'how', 'or', 'more', 'there', 'like', 'was', 'then', 'many', 'our', 'them', 'into', 'been', 'has'
])

# Membersihkan teks
cleaned_text = re.sub(r'\W', ' ', text)
cleaned_text = re.sub(r'\s+', ' ', cleaned_text)
cleaned_text = cleaned_text.lower()

# Tokenisasi kata dengan metode split
tokens = cleaned_text.split()

# Menghilangkan stopwords
filtered_tokens = [word for word in tokens if word not in stop_words]

# Gabungkan kata kembali menjadi teks
processed_text = ' '.join(filtered_tokens)

# Membuat wordcloud
wordcloud = WordCloud(width=800, height=400, background_color='white').generate(processed_text)

# Menampilkan wordcloud
plt.figure(figsize=(10, 5))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.show()

# Hitung frekuensi kata
word_freq = Counter(filtered_tokens)

# Ambil 20 kata yang paling sering muncul
common_words = word_freq.most_common(20)

# Membuat DataFrame
df_common_words = pd.DataFrame(common_words, columns=['Word', 'Frequency'])

# Membuat barplot dengan matplotlib
plt.figure(figsize=(10, 5))
plt.bar(df_common_words['Word'], df_common_words['Frequency'])
plt.xticks(rotation=45)
plt.title('Top 20 Most Common Words')
plt.xlabel('Words')
plt.ylabel('Frequency')
plt.show()

# Analisis sentimen sederhana
positive_words = ['good', 'great', 'excellent', 'positive', 'fortunate', 'correct', 'superior']
negative_words = ['bad', 'worst', 'negative', 'unfortunate', 'wrong', 'inferior']

positive_count = sum([1 for word in filtered_tokens if word in positive_words])
negative_count = sum([1 for word in filtered_tokens if word in negative_words])

sentiment_score = positive_count - negative_count

# Menampilkan hasil analisis sentimen
print(f'Positive words: {positive_count}')
print(f'Negative words: {negative_count}')
print(f'Sentiment score: {sentiment_score}')
