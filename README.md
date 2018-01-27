

{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Python Assignment\n",
    "\n",
    "\n",
    "|Name|examnr.|\n",
    "|----|-------|\n",
    "|Ying Luo|2007491|\n",
    "|Borja Casado|2016493|"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Question\n",
    "\n",
    "Is the issue of gender inequality important enough to talk about on Twitter in India?\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Motivation\n",
    "Gender Inequality, in simple words, may be defined as discrimination against women based on their sex. This peculiar type of discrimination against women is prevalent everywhere in the world and more so in Indian society. According to the Global Gender Gap Report released by the [World Economic Forum](https://www.weforum.org/events/world-economic-forum-annual-meeting-2018) (WEF) in 2014, India was ranked 114 on the Gender Gap Index (GGI) among 142 countries polled. When broken down into components of the GGI, India performs well on political empowerment, but is scored bad on Economic participation(134th) and opportunity, India also scores poorly on overall female to male literacy (126th) and health rankings (141th).\n",
    "\n",
    "The root cause of gender inequality in Indian society lies in its patriarchy system. According to the famous sociologists Sylvia Walby, patriarchy is \"a system of social structure and practices in which men dominate, oppress and exploit women\". Women's exploitation is an age old cultural phenomenon of Indian society. The system of patriarchy finds its validity and sanction in our religious beliefs, whether it is Hindu, Muslim or any other religion.\n",
    "\n",
    "Although there has been an increasing focus on women's empowerment in India, especially on how to economically empower women through education and work but on the ground there are not enough visible changes. The change will appear only when the mind set of Indian society changes; when society starts treating male and female on equal footing and when a girl is not considered as a burden. Twitter, as one of the most popular social network, there were 23.2 million ([Statista](https://www.statista.com/statistics/381832/twitter-users-india/)) monthly active users in India, we would like to see whether this is an active issue in Twitter, by interpreting India's trending topics in order to see what people is saying about #feminism and by looking at two related indicators from [The World Bank](https://data.worldbank.org/).\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Method\n",
    "\n",
    "To get a simple but clear picture of woman's empowerment in india, we request information from World Bank data API’s, [wbdata](http://wbdata.readthedocs.io/en/latest/) to obtain Labour force female (% of total labour force) and Secondary education, pupils (% female). The first indicator shows us women's perfomance in the labour market and the second gives us an understanding of the proportion of women in secondary education, which is the most crucial for them to progress and become more powerful.\n",
    "\n",
    "As we know, social media is becoming more and more important when it comes to communication and knowledge. Twitter, is a free network where everybody can express their thoughts and therefore is very useful when we want to interpret people behaviour by looking at trending topics. We can infer some conclusions given some keywords that people use more often or given the popularity of certain twit or personality, and that is what we did here. We first need to access the data. In order to mine data from [Twitter](https://twitter.com/), we first created a developer app in [Twitter API](https://apps.twitter.com/) so that we could get access. Then, we chose the country we are interested in by inserting the country's [Where On Earth ID](http://woeid.rosselliot.co.nz/) and then after acquiring India's trending topics, we filtered them by the keyword #feminism in order to see what people talk about it. We interpreted how popular feminism is in twitter by looking at the trending topics, hashtags as well as who is popular when it comes to talk about feminism.  \n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Answer\n",
    "\n",
    "Yes, indian people do care about feminism. India is a country where many things have to be done yet and gender equality is one of them. However, we learn from the world bank data that its secondary education is improving over time and that will lead to a better women perfomance in the labour market. On the other hand, the labour market still show a big gap between men and women and the wish to make changes is captured in twitter through people claims when they talk about #feminism. \n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Extracting Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 65,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "#Libraries imported\n",
    "\n",
    "import wbdata \n",
    "import pandas as pd \n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "import twitter\n",
    "from prettytable import PrettyTable\n",
    "import datetime \n",
    "import requests\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's first have a look at the indicators that show a clear picture of the women's perfomance in India."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "#set up the country we want\n",
    "\n",
    "countries = [\"IND\"]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "#set up the indicator we want by building a dictionary\n",
    "\n",
    "indicators = {'SL.TLF.TOTL.FE.ZS':'Labour force female', 'SE.SEC.ENRL.FE.ZS':'Sec. Ed. Pupils% Female'}\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 60,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": [
    "#grab indicators above for India and load into a data frame\n",
    "\n",
    "df = wbdata.get_dataframe(indicators, country=countries, convert_date=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 61,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "      Labour force female\n",
      "date                     \n",
      "2016            24.693361\n",
      "2015            24.521093\n",
      "2014            24.378622\n",
      "2013            24.235051\n",
      "2012            24.097772\n",
      "2011            24.563874\n",
      "2010            25.037949\n",
      "2009            25.920097\n",
      "2008            26.805637\n",
      "2007            27.691578\n",
      "      Sec. Ed. Pupils% Female\n",
      "date                         \n",
      "2016                      NaN\n",
      "2015                47.599850\n",
      "2014                47.645618\n",
      "2013                47.573021\n",
      "2012                46.148151\n",
      "2011                45.981621\n",
      "2010                45.621319\n",
      "2009                45.494831\n",
      "2008                44.578659\n",
      "2007                43.972580\n"
     ]
    }
   ],
   "source": [
    "#To make it more relevant, we get data for the last 10 years\n",
    "\n",
    "ind_lff = df.iloc[[1,2,3,4,5,6,7,8,9,10],[0]] #India labour force indicator for the past 10 years\n",
    "ind_se = df.iloc[[1,2,3,4,5,6,7,8,9,10],[1]] #India secondary education indicator for the past 10 years\n",
    "\n",
    "print(ind_lff)\n",
    "print(ind_se)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "As the data comes in downwards trend, we need to manipulate it and revert it so that the following plots make sense.."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 62,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "      Labour force female\n",
      "date                     \n",
      "2007            27.691578\n",
      "2008            26.805637\n",
      "2009            25.920097\n",
      "2010            25.037949\n",
      "2011            24.563874\n",
      "2012            24.097772\n",
      "2013            24.235051\n",
      "2014            24.378622\n",
      "2015            24.521093\n",
      "2016            24.693361\n",
      "      Sec. Ed. Pupils% Female\n",
      "date                         \n",
      "2007                43.972580\n",
      "2008                44.578659\n",
      "2009                45.494831\n",
      "2010                45.621319\n",
      "2011                45.981621\n",
      "2012                46.148151\n",
      "2013                47.573021\n",
      "2014                47.645618\n",
      "2015                47.599850\n",
      "2016                      NaN\n"
     ]
    }
   ],
   "source": [
    "#Correcting the trend by using reindex.\n",
    "\n",
    "ind_lff = ind_lff.sort_index(axis=1 ,ascending=True)\n",
    "ind_lff = ind_lff.reindex(index=ind_lff.index[::-1])\n",
    "ind_se = ind_se.sort_index(axis=1 ,ascending=True)\n",
    "ind_se = ind_se.reindex(index=ind_se.index[::-1])\n",
    "\n",
    "print(ind_lff)\n",
    "print(ind_se)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "And we finally plot the customized graphs."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 91,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n3bp1s3ocTyrOVSOzZ8+mSZMmtGvXjpSu110NZ2b89NNPzJ49m/bt22f1WH77y7lqZOXKlTRv3twT\niitGEs2bN89JCbZaJZVffok7Aufi5wnFlSZX50W1SiozZ8KXX8YdhXPO1VzVKqkUFcFxx8Hy5XFH\n4lzN1bhx44pXigwePJhbb701i9EECxYsoGfPnnTt2pX33y9rVOjMuvPOO9l555056aSTsnaMXH1/\nlVHtKuqnTYOBA+Gxx+KOxDkXl3Xr1lG7dnJY+HfeeYeddtqJJ554YoP3UVn33nsvr7/+etYrxvNN\ntSqpJDz+ODz6aIWrOVdtrV4dbgVn67W61DFAy/avf/1rfUnhwAMPZN685NDvU6ZM4YADDqBDhw48\n9NBDQGitdPnll7PbbrvRsWNHRowIg0aOGTOGI444Yv225513Ho8//jgQumkaMmQIe++9N6NGjVq/\nzuTJkxk0aBCvvfYaXbp0YcWKFQwfPpyOHTuy22678ec//3n9uo0bN+aaa66hZ8+efPTRR4wfP57f\n/va3dO7cmR49elBYWMi6deu4/PLL6d69O506deKBB349gOfZZ5/NN998w1FHHcXQoUNZtmwZp512\nGt27d6dr16689NJLADz++OP07t2bI488kvbt23P33Xdz++2307VrV/bYYw9+/vlnAB566CG6d+9O\n586dOfbYY1leyu2Y//3vfxxyyCF069aNffbZhy+++KJyf6RMMbNq86pTp5uBGZg1aGA2ebI5V6N8\n/vnnZmY2Y4at/7+QjdeMGWXH0KhRo1/N+/nnn62oqMjMzB566CG75JJLzMzs2muvtU6dOtny5ctt\nwYIF1rp1a5szZ449++yzduCBB9ratWvtxx9/tDZt2tgPP/xgo0ePtsMPP3z9fgcOHGiPPfaYmZm1\nbdvW/v73v5ca02OPPWYDBw40M7M5c+ZYmzZtbP78+bZmzRrr1auXvfDCC2ZmBtiIESPMzGzVqlXW\nvn17GzdunJmZLV682NasWWMPPPCAXXfddWZmtnLlSuvWrZt98803vzpm27ZtbcGCBWZmdsUVV9hT\nTz1lZma//PKLdejQwZYuXWqPPfaYbb/99rZkyRKbP3++NW3a1O677z4zM7vooots6NChZma2cOHC\n9fu98sor7c4771z//d1yyy1mZnbAAQfYl19+aWZmH3/8sfXq1etXMSXOj1TABMvk73A8qSw7ttsO\nvv461K2sXBnqVyZMgGaVHtHcOZdJs2fPpk+fPsydO5fVq1cXuyV09NFH07BhQxo2bEivXr0YN24c\nH3zwAf369aN27dpstdVW7LfffowfP56mTZuWe5w+ffpUGMv48ePZf//9adEidMx70kkn8d5779G7\nd29q167NscceC8CMGTPYZptt6N69O8D6Y7/55ptMnTqVZ599FoDFixfz1VdflXub68033+Tll19e\nX/+xcuVKvv/+ewB69epFkyZNaNKkCc2aNePII48EoGPHjkydOhWATz/9lKuuuopFixaxdOlSDj74\n4GL7X7p0KR9++CHHH3/8+nmrVq2q8LvIhmqVVJo0gSFD4KqrwvTXX8Npp8Gzz4K3snQuPueffz6X\nXHIJRx11FGPGjGHw4MHrl5Vs6ioJK2OY8zp16lBUVLR+uuRzF40aNaowlrL2DeGp80Q9ipmV2gzX\nzLjrrrt+9cNe0TGfe+45dtxxx2Lzx44dS/369ddP16pVa/10rVq1WLt2LQCnnHIKL774Ip07d+bx\nxx9nzJgxxfZTVFTEpptuyuTJk9OOKVuqXZ3KFVfAoYcmp59/Hu64I754nItDu3YwY0b2XpUdtmjx\n4sW0atUK4FeV5S+99BIrV67kp59+YsyYMXTv3p19992XESNGsG7dOhYsWMB7771Hjx49aNu2LZ9/\n/jmrVq1i8eLFvPPOO5X+bnr27Mm7777LwoULWbduHcOHD2e//fb71Xo77bQTP/zwA+PHjwegsLCQ\ntWvXcvDBB3PfffexZs0aAL788kuWLVtW7jEPPvhg7rrrrvUJ7ZNPPqlUzIWFhWyzzTasWbOGYcOG\n/Wp506ZNad++/fq6JDNjypQplTpGplSrkgpArVrw1FOw++4QlS65/HLo2RP23DPe2JzLlXr14De/\niefYy5cvp3Xr1uunL7nkEgYPHszxxx9Pq1at2GOPPfj222/XL+/RoweHH34433//PVdffTUtW7bk\nmGOO4aOPPqJz585I4uabb2brrbcG4IQTTqBTp0506NCBrl27Vjq+bbbZhhtvvJFevXphZhx22GEc\nffTRv1qvXr16jBgxgvPPP58VK1bQsGFD3n77bc444wxmzpzJ7rvvjpnRokULXnzxxXKPefXVV3PR\nRRfRqVMnzIx27drxyiuvpB3zddddR8+ePWnbti0dO3aksLDwV+sMGzaMc845h//7v/9jzZo19O3b\n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\n3EpkAAAAAElFTkSuQmCC\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x17e280127b8>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "ind_lff.plot(color=\"blue\", linewidth=3.3)\n",
    "plt.rcParams['axes.facecolor'] = 'white'\n",
    "plt.title(\"Labour force female (% of total labour force)\")\n",
    "plt.ylabel(\"Percentage\")\n",
    "plt.xlabel(\"Year\")\n",
    "plt.xticks(rotation=45)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 92,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\nJIHa3BYtCrWZTDELx1xef+cfaxgJZdYsWI+mm7R9+ilsu+06i3v16sWwYcPYdtttOfDAA+nfvz/7\n7bcfq1ev5txzz+WFF16gY8eOPPHEEwwdOpT777+fAQMGcOmll3LkkUeyYsUK1qbx62rAgAE/NXkd\ndNBB3HjjjZx//vmceeaZnHjiidxxxx21fkp///vf6dGjR43bTZs2jffee48ff/yRXXfdlT59+lS5\n7U033cQdd9xBz549Wbp0KYWFhZx99tmceOKJrFy5knvuuYdhw4YxdOjQGju3Bw8ezPDhYSbjhx56\niE6dOjF8+HDGjx9Py5YtueGGG/jzn//MlVdeCUDXrl2ZNGkSf/jDHzj55JN55513WLFiBd27d2fQ\noEEUFhby3HPP0aZNG7799lv22msv+vXrVyGOV155hc8//5z3338fM6Nfv35MnDiRfffdN52XtP4p\nK4ODDoLp0zNzvPbtq08UXbpAhw6161Avb6Jq2zb88KsNs4rJaNGi2iWksrLUx23XrnZx1CENI6HE\npFWrVkyePJm33nqLN954g/79+zNy5EhKSkr4+OOPOeiggwBYs2YNnTp1YsmSJcydO5cjjzwSCCfJ\npSNVk9c777zDM888A8AJJ5zAJZdcktaxypNTcXExt912W43bH3744TRv3pzmzZtzwAEH8P7777PL\nLruk3LZnz55ceOGFDBgwgN/85jcUFRXRrVs3JkyYAMDMmTOZN28e22+/PSeccAKrVq3i2muvZdsU\nybpyk9e4ceOYPn06PXv2BGDVqlXsvffeP63v168fAD169GDp0qW0bt2a1q1bU1hYyKJFi2jZsiWX\nXXYZEydOpFGjRsydO5evv/6azTbb7KdjvPLKK7zyyivsuuuuACxdupTPP/+84SaUxx5LL5k0bbpu\nYkhVq0jz854zErRpE26bb167fc3gxx9TJ5pu3bITbx7IekKRVACUAnPNrK+kt4DW0epNgPfN7IgU\n+60BPoqKs82sX7ZjzYaCggL2339/9t9/f3r06MGDDz7I7rvvTvfu3Zk0aVKFbRcvXpzRx16fIayp\nklPjxo1/qilVPsei8mNU95iXXnopffr04cUXX2SvvfZi/PjxbL99YtbYoUOHMnz4cG699VYGDBhA\ncXEx11xzDY888kiNcZsZBx10EI899ljK9eXNjo0aNfrpfnm5rKyMRx55hAULFjB58mSaNGlCcXHx\n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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x17e29806f98>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "ind_se.plot(color=\"red\", linewidth=3.3)\n",
    "plt.rcParams['axes.facecolor'] = 'white'\n",
    "plt.title(\"Secondary education, pupils (% female)\")\n",
    "plt.ylabel(\"Percentage\")\n",
    "plt.xlabel(\"Year\")\n",
    "plt.xticks(rotation=45)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "As we can see from the first graph that women's performance in the labour market has been going worse despite of a slight increase in recent years. It implies that men are still the vast majority at work and there is a long way to go for woman to be financially independent and equal.\n",
    "\n",
    "The second graph shows that proportion of female pupils in secondary education has been improving significantly since 2012. This tells us that the progress to gender equality has been made as education is the first pillar that should be improved before expecting any change in the labour market.\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's now begin to extract the data from Twitter for the trending topics of today 27-01-2018"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<twitter.api.Twitter object at 0x0000017E27C330F0>\n"
     ]
    }
   ],
   "source": [
    "#In order to access to Twitter data, we have to acquire the convenient developer credentials by using Twitter API.\n",
    "\n",
    "CONSUMER_KEY = ''\n",
    "CONSUMER_SECRET = ''\n",
    "OAUTH_TOKEN = ''\n",
    "OAUTH_TOKEN_SECRET = ''\n",
    "\n",
    "auth = twitter.oauth.OAuth(OAUTH_TOKEN, OAUTH_TOKEN_SECRET,\n",
    "                           CONSUMER_KEY, CONSUMER_SECRET)\n",
    "\n",
    "twitter_api = twitter.Twitter(auth=auth)\n",
    "\n",
    "print (twitter_api)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[{'trends': [{'name': '#Kasganj', 'url': 'http://twitter.com/search?q=%23Kasganj', 'promoted_content': None, 'query': '%23Kasganj', 'tweet_volume': 28530}, {'name': 'Shami', 'url': 'http://twitter.com/search?q=Shami', 'promoted_content': None, 'query': 'Shami', 'tweet_volume': 16510}, {'name': '#Kabul', 'url': 'http://twitter.com/search?q=%23Kabul', 'promoted_content': None, 'query': '%23Kabul', 'tweet_volume': 14312}, {'name': '#MuniTarunSagarwithJC', 'url': 'http://twitter.com/search?q=%23MuniTarunSagarwithJC', 'promoted_content': None, 'query': '%23MuniTarunSagarwithJC', 'tweet_volume': None}, {'name': '#KERDEL', 'url': 'http://twitter.com/search?q=%23KERDEL', 'promoted_content': None, 'query': '%23KERDEL', 'tweet_volume': None}, {'name': 'Dean Elgar', 'url': 'http://twitter.com/search?q=%22Dean+Elgar%22', 'promoted_content': None, 'query': '%22Dean+Elgar%22', 'tweet_volume': None}, {'name': '#BhaagamathieMakingVideo', 'url': 'http://twitter.com/search?q=%23BhaagamathieMakingVideo', 'promoted_content': None, 'query': '%23BhaagamathieMakingVideo', 'tweet_volume': None}, {'name': 'Basil Thampi', 'url': 'http://twitter.com/search?q=%22Basil+Thampi%22', 'promoted_content': None, 'query': '%22Basil+Thampi%22', 'tweet_volume': None}, {'name': 'Ishant Sharma', 'url': 'http://twitter.com/search?q=%22Ishant+Sharma%22', 'promoted_content': None, 'query': '%22Ishant+Sharma%22', 'tweet_volume': None}, {'name': 'Grand Slam', 'url': 'http://twitter.com/search?q=%22Grand+Slam%22', 'promoted_content': None, 'query': '%22Grand+Slam%22', 'tweet_volume': 40894}, {'name': '#Shopian', 'url': 'http://twitter.com/search?q=%23Shopian', 'promoted_content': None, 'query': '%23Shopian', 'tweet_volume': None}, {'name': '#KarthikSrinivasanCalendar', 'url': 'http://twitter.com/search?q=%23KarthikSrinivasanCalendar', 'promoted_content': None, 'query': '%23KarthikSrinivasanCalendar', 'tweet_volume': None}, {'name': '#SaintRamRahim_Initiative82', 'url': 'http://twitter.com/search?q=%23SaintRamRahim_Initiative82', 'promoted_content': None, 'query': '%23SaintRamRahim_Initiative82', 'tweet_volume': None}, {'name': '#HarryANDSejalOnANDPictures', 'url': 'http://twitter.com/search?q=%23HarryANDSejalOnANDPictures', 'promoted_content': None, 'query': '%23HarryANDSejalOnANDPictures', 'tweet_volume': None}, {'name': '#WanderersTest', 'url': 'http://twitter.com/search?q=%23WanderersTest', 'promoted_content': None, 'query': '%23WanderersTest', 'tweet_volume': None}, {'name': '#IndiaWithJamidha', 'url': 'http://twitter.com/search?q=%23IndiaWithJamidha', 'promoted_content': None, 'query': '%23IndiaWithJamidha', 'tweet_volume': None}, {'name': '#TwitterBestFandom', 'url': 'http://twitter.com/search?q=%23TwitterBestFandom', 'promoted_content': None, 'query': '%23TwitterBestFandom', 'tweet_volume': 6818314}, {'name': '#BBK5FinaleWithSudeep', 'url': 'http://twitter.com/search?q=%23BBK5FinaleWithSudeep', 'promoted_content': None, 'query': '%23BBK5FinaleWithSudeep', 'tweet_volume': None}, {'name': '#Neerali', 'url': 'http://twitter.com/search?q=%23Neerali', 'promoted_content': None, 'query': '%23Neerali', 'tweet_volume': None}, {'name': '#BattiGulMeterChalu', 'url': 'http://twitter.com/search?q=%23BattiGulMeterChalu', 'promoted_content': None, 'query': '%23BattiGulMeterChalu', 'tweet_volume': None}, {'name': '#Palestine', 'url': 'http://twitter.com/search?q=%23Palestine', 'promoted_content': None, 'query': '%23Palestine', 'tweet_volume': None}, {'name': '#BjpVijaySankalp2018', 'url': 'http://twitter.com/search?q=%23BjpVijaySankalp2018', 'promoted_content': None, 'query': '%23BjpVijaySankalp2018', 'tweet_volume': None}, {'name': '#TouchChesiChuduPreRelease', 'url': 'http://twitter.com/search?q=%23TouchChesiChuduPreRelease', 'promoted_content': None, 'query': '%23TouchChesiChuduPreRelease', 'tweet_volume': None}, {'name': '#Budget2018WithTimesNow', 'url': 'http://twitter.com/search?q=%23Budget2018WithTimesNow', 'promoted_content': None, 'query': '%23Budget2018WithTimesNow', 'tweet_volume': None}, {'name': '#SLvBAN', 'url': 'http://twitter.com/search?q=%23SLvBAN', 'promoted_content': None, 'query': '%23SLvBAN', 'tweet_volume': None}, {'name': '#InttelligentTeaser', 'url': 'http://twitter.com/search?q=%23InttelligentTeaser', 'promoted_content': None, 'query': '%23InttelligentTeaser', 'tweet_volume': None}, {'name': '#CCFCvARW', 'url': 'http://twitter.com/search?q=%23CCFCvARW', 'promoted_content': None, 'query': '%23CCFCvARW', 'tweet_volume': None}, {'name': '#ETMagazine', 'url': 'http://twitter.com/search?q=%23ETMagazine', 'promoted_content': None, 'query': '%23ETMagazine', 'tweet_volume': None}, {'name': '#LetsDineout', 'url': 'http://twitter.com/search?q=%23LetsDineout', 'promoted_content': None, 'query': '%23LetsDineout', 'tweet_volume': None}, {'name': '#Kohli', 'url': 'http://twitter.com/search?q=%23Kohli', 'promoted_content': None, 'query': '%23Kohli', 'tweet_volume': None}, {'name': '#ShubhMangalSaavdhan', 'url': 'http://twitter.com/search?q=%23ShubhMangalSaavdhan', 'promoted_content': None, 'query': '%23ShubhMangalSaavdhan', 'tweet_volume': None}, {'name': '#ChandrababuNaidu', 'url': 'http://twitter.com/search?q=%23ChandrababuNaidu', 'promoted_content': None, 'query': '%23ChandrababuNaidu', 'tweet_volume': None}, {'name': '#Jeeyar', 'url': 'http://twitter.com/search?q=%23Jeeyar', 'promoted_content': None, 'query': '%23Jeeyar', 'tweet_volume': None}, {'name': '#MaduraVeeranFromFeb2nd', 'url': 'http://twitter.com/search?q=%23MaduraVeeranFromFeb2nd', 'promoted_content': None, 'query': '%23MaduraVeeranFromFeb2nd', 'tweet_volume': None}, {'name': '#TheTallManBijuPatnaik', 'url': 'http://twitter.com/search?q=%23TheTallManBijuPatnaik', 'promoted_content': None, 'query': '%23TheTallManBijuPatnaik', 'tweet_volume': None}], 'as_of': '2018-01-27T15:43:19Z', 'created_at': '2018-01-27T15:42:17Z', 'locations': [{'name': 'India', 'woeid': 23424848}]}]\n"
     ]
    }
   ],
   "source": [
    "#India's Where On Earth ID\n",
    "\n",
    "IN_WOE_ID = 23424848 #India\n",
    "\n",
    "#And then we list the trends so that we can know who influence more by keywords\n",
    "\n",
    "in_trends = twitter_api.trends.place(_id=IN_WOE_ID)\n",
    "\n",
    "#We print the lists out\n",
    "\n",
    "print(in_trends)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "As the output is printed out as Native Python data structures, we should import JSON in order to convert the text in pretty printing (more readable)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[\n",
      " {\n",
      "  \"as_of\": \"2018-01-27T15:43:19Z\",\n",
      "  \"created_at\": \"2018-01-27T15:42:17Z\",\n",
      "  \"locations\": [\n",
      "   {\n",
      "    \"name\": \"India\",\n",
      "    \"woeid\": 23424848\n",
      "   }\n",
      "  ],\n",
      "  \"trends\": [\n",
      "   {\n",
      "    \"name\": \"#Kasganj\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Kasganj\",\n",
      "    \"tweet_volume\": 28530,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Kasganj\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"Shami\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"Shami\",\n",
      "    \"tweet_volume\": 16510,\n",
      "    \"url\": \"http://twitter.com/search?q=Shami\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Kabul\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Kabul\",\n",
      "    \"tweet_volume\": 14312,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Kabul\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#MuniTarunSagarwithJC\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23MuniTarunSagarwithJC\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23MuniTarunSagarwithJC\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#KERDEL\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23KERDEL\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23KERDEL\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"Dean Elgar\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%22Dean+Elgar%22\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%22Dean+Elgar%22\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#BhaagamathieMakingVideo\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23BhaagamathieMakingVideo\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23BhaagamathieMakingVideo\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"Basil Thampi\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%22Basil+Thampi%22\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%22Basil+Thampi%22\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"Ishant Sharma\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%22Ishant+Sharma%22\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%22Ishant+Sharma%22\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"Grand Slam\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%22Grand+Slam%22\",\n",
      "    \"tweet_volume\": 40894,\n",
      "    \"url\": \"http://twitter.com/search?q=%22Grand+Slam%22\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Shopian\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Shopian\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Shopian\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#KarthikSrinivasanCalendar\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23KarthikSrinivasanCalendar\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23KarthikSrinivasanCalendar\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#SaintRamRahim_Initiative82\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23SaintRamRahim_Initiative82\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23SaintRamRahim_Initiative82\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#HarryANDSejalOnANDPictures\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23HarryANDSejalOnANDPictures\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23HarryANDSejalOnANDPictures\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#WanderersTest\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23WanderersTest\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23WanderersTest\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#IndiaWithJamidha\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23IndiaWithJamidha\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23IndiaWithJamidha\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#TwitterBestFandom\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23TwitterBestFandom\",\n",
      "    \"tweet_volume\": 6818314,\n",
      "    \"url\": \"http://twitter.com/search?q=%23TwitterBestFandom\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#BBK5FinaleWithSudeep\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23BBK5FinaleWithSudeep\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23BBK5FinaleWithSudeep\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Neerali\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Neerali\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Neerali\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#BattiGulMeterChalu\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23BattiGulMeterChalu\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23BattiGulMeterChalu\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Palestine\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Palestine\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Palestine\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#BjpVijaySankalp2018\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23BjpVijaySankalp2018\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23BjpVijaySankalp2018\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#TouchChesiChuduPreRelease\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23TouchChesiChuduPreRelease\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23TouchChesiChuduPreRelease\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Budget2018WithTimesNow\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Budget2018WithTimesNow\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Budget2018WithTimesNow\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#SLvBAN\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23SLvBAN\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23SLvBAN\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#InttelligentTeaser\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23InttelligentTeaser\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23InttelligentTeaser\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#CCFCvARW\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23CCFCvARW\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23CCFCvARW\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#ETMagazine\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23ETMagazine\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23ETMagazine\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#LetsDineout\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23LetsDineout\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23LetsDineout\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Kohli\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Kohli\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Kohli\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#ShubhMangalSaavdhan\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23ShubhMangalSaavdhan\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23ShubhMangalSaavdhan\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#ChandrababuNaidu\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23ChandrababuNaidu\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23ChandrababuNaidu\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#Jeeyar\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23Jeeyar\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23Jeeyar\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#MaduraVeeranFromFeb2nd\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23MaduraVeeranFromFeb2nd\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23MaduraVeeranFromFeb2nd\"\n",
      "   },\n",
      "   {\n",
      "    \"name\": \"#TheTallManBijuPatnaik\",\n",
      "    \"promoted_content\": null,\n",
      "    \"query\": \"%23TheTallManBijuPatnaik\",\n",
      "    \"tweet_volume\": null,\n",
      "    \"url\": \"http://twitter.com/search?q=%23TheTallManBijuPatnaik\"\n",
      "   }\n",
      "  ]\n",
      " }\n",
      "]\n"
     ]
    }
   ],
   "source": [
    "#Importing JSON\n",
    "\n",
    "import json\n",
    "\n",
    "#Printing a more readable version of the lists of trends and we sort each trend by using the option True in sort_keys\n",
    "\n",
    "\n",
    "print (json.dumps(in_trends, indent=1, sort_keys=True))\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Length of statuses\n",
      "{\n",
      " \"created_at\": \"Sat Jan 27 15:42:00 +0000 2018\",\n",
      " \"id\": 957277519278874624,\n",
      " \"id_str\": \"957277519278874624\",\n",
      " \"text\": \"Pakistani feminist banned by @Facebook for criticizing those who victim blamed (on religious grounds) a 7-year old\\u2026 https://t.co/rNDkGyD9jM\",\n",
      " \"truncated\": true,\n",
      " \"entities\": {\n",
      "  \"hashtags\": [],\n",
      "  \"symbols\": [],\n",
      "  \"user_mentions\": [\n",
      "   {\n",
      "    \"screen_name\": \"facebook\",\n",
      "    \"name\": \"Facebook\",\n",
      "    \"id\": 2425151,\n",
      "    \"id_str\": \"2425151\",\n",
      "    \"indices\": [\n",
      "     29,\n",
      "     38\n",
      "    ]\n",
      "   }\n",
      "  ],\n",
      "  \"urls\": [\n",
      "   {\n",
      "    \"url\": \"https://t.co/rNDkGyD9jM\",\n",
      "    \"expanded_url\": \"https://twitter.com/i/web/status/957277519278874624\",\n",
      "    \"display_url\": \"twitter.com/i/web/status/9\\u2026\",\n",
      "    \"indices\": [\n",
      "     116,\n",
      "     139\n",
      "    ]\n",
      "   }\n",
      "  ]\n",
      " },\n",
      " \"metadata\": {\n",
      "  \"iso_language_code\": \"en\",\n",
      "  \"result_type\": \"recent\"\n",
      " },\n",
      " \"source\": \"<a href=\\\"https://about.twitter.com/products/tweetdeck\\\" rel=\\\"nofollow\\\">TweetDeck</a>\",\n",
      " \"in_reply_to_status_id\": null,\n",
      " \"in_reply_to_status_id_str\": null,\n",
      " \"in_reply_to_user_id\": null,\n",
      " \"in_reply_to_user_id_str\": null,\n",
      " \"in_reply_to_screen_name\": null,\n",
      " \"user\": {\n",
      "  \"id\": 439528396,\n",
      "  \"id_str\": \"439528396\",\n",
      "  \"name\": \"Golan Shahar \\u00ae \\ud83c\\uddee\\ud83c\\uddf1\\u2721\",\n",
      "  \"screen_name\": \"GolanSh\",\n",
      "  \"location\": \"#Jerusalem, #Israel\",\n",
      "  \"description\": \"The proud father of Maya. #Zionist #Israeli #Jew #USA \\ud83c\\uddfa\\ud83c\\uddf8\\u00a0Curious \\ud83d\\udd2c\\ud83d\\udcda #News #History #Kurdistan #NotPC Stay away from my Jew Gold! RT \\u2260 endorsement \\ud83c\\uddec\\ud83c\\udde7\\ud83c\\udde8\\ud83c\\udde6\\ud83c\\udde6\\ud83c\\uddfa\\ud83c\\uddf3\\ud83c\\uddff\\ud83c\\uddee\\ud83c\\uddf3\",\n",
      "  \"url\": \"https://t.co/LmNQt0E959\",\n",
      "  \"entities\": {\n",
      "   \"url\": {\n",
      "    \"urls\": [\n",
      "     {\n",
      "      \"url\": \"https://t.co/LmNQt0E959\",\n",
      "      \"expanded_url\": \"http://blog.golanshahar.com\",\n",
      "      \"display_url\": \"blog.golanshahar.com\",\n",
      "      \"indices\": [\n",
      "       0,\n",
      "       23\n",
      "      ]\n",
      "     }\n",
      "    ]\n",
      "   },\n",
      "   \"description\": {\n",
      "    \"urls\": []\n",
      "   }\n",
      "  },\n",
      "  \"protected\": false,\n",
      "  \"followers_count\": 13485,\n",
      "  \"friends_count\": 11930,\n",
      "  \"listed_count\": 311,\n",
      "  \"created_at\": \"Sat Dec 17 22:11:16 +0000 2011\",\n",
      "  \"favourites_count\": 8868,\n",
      "  \"utc_offset\": 7200,\n",
      "  \"time_zone\": \"Jerusalem\",\n",
      "  \"geo_enabled\": true,\n",
      "  \"verified\": false,\n",
      "  \"statuses_count\": 92420,\n",
      "  \"lang\": \"en\",\n",
      "  \"contributors_enabled\": false,\n",
      "  \"is_translator\": false,\n",
      "  \"is_translation_enabled\": false,\n",
      "  \"profile_background_color\": \"FFFFFF\",\n",
      "  \"profile_background_image_url\": \"http://pbs.twimg.com/profile_background_images/606207598367367168/65XefGl-.jpg\",\n",
      "  \"profile_background_image_url_https\": \"https://pbs.twimg.com/profile_background_images/606207598367367168/65XefGl-.jpg\",\n",
      "  \"profile_background_tile\": true,\n",
      "  \"profile_image_url\": \"http://pbs.twimg.com/profile_images/859073767942836224/v9XnziNV_normal.jpg\",\n",
      "  \"profile_image_url_https\": \"https://pbs.twimg.com/profile_images/859073767942836224/v9XnziNV_normal.jpg\",\n",
      "  \"profile_banner_url\": \"https://pbs.twimg.com/profile_banners/439528396/1408557762\",\n",
      "  \"profile_link_color\": \"1B95E0\",\n",
      "  \"profile_sidebar_border_color\": \"FFFFFF\",\n",
      "  \"profile_sidebar_fill_color\": \"DDEEF6\",\n",
      "  \"profile_text_color\": \"333333\",\n",
      "  \"profile_use_background_image\": true,\n",
      "  \"has_extended_profile\": true,\n",
      "  \"default_profile\": false,\n",
      "  \"default_profile_image\": false,\n",
      "  \"following\": false,\n",
      "  \"follow_request_sent\": false,\n",
      "  \"notifications\": false,\n",
      "  \"translator_type\": \"regular\"\n",
      " },\n",
      " \"geo\": null,\n",
      " \"coordinates\": null,\n",
      " \"place\": null,\n",
      " \"contributors\": null,\n",
      " \"is_quote_status\": false,\n",
      " \"retweet_count\": 1,\n",
      " \"favorite_count\": 1,\n",
      " \"favorited\": false,\n",
      " \"retweeted\": false,\n",
      " \"possibly_sensitive\": false,\n",
      " \"lang\": \"en\"\n",
      "}\n"
     ]
    }
   ],
   "source": [
    "#We set our keyword variableOur keyword is #feminism\n",
    "\n",
    "q = '#feminism'\n",
    "\n",
    "count = 100\n",
    "\n",
    "search_results = twitter_api.search.tweets(q=q, count=count)\n",
    "\n",
    "statuses = search_results['statuses']\n",
    "\n",
    "# We set a loop with the aim foriterate through 5 more batches of results by following the cursor\n",
    "\n",
    "for _ in range(5):\n",
    "    print (\"Length of statuses\"), len(statuses)\n",
    "    try:\n",
    "        next_results = search_results['search_metadata']['next_results']\n",
    "    except KeyError: # No more results when next_results doesn't exist\n",
    "        break\n",
    "        \n",
    "    # Create a dictionary from next_results\n",
    "    \n",
    "    kwargs = dict([ kv.split('=') for kv in next_results[1:].split(\"&\") ])\n",
    "    \n",
    "    search_results = twitter_api.search.tweets(**kwargs)\n",
    "    statuses += search_results['statuses']\n",
    "\n",
    "# Show one sample search result by slicing the list to see what we have\n",
    "\n",
    "print (json.dumps(statuses[0], indent=1))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We collect screen names, hashtags and status:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[\n",
      " \"Pakistani feminist banned by @Facebook for criticizing those who victim blamed (on religious grounds) a 7-year old\\u2026 https://t.co/rNDkGyD9jM\",\n",
      " \"RT @WomenThrive: The discussions on #feminism, #MeToo , #equalpay, #genderinequality etc. at #WEF2018 are a huge step for gender equality w\\u2026\",\n",
      " \"\\\"Teach your daughters less about fitting into glass slippers and more about shattering glass ceilings\\\".\\u2026 https://t.co/0n5j3DnZ1P\",\n",
      " \"\\u201cEnsinaste-me que cada um de n\\u00f3s vem a este mundo para ganhar experi\\u00eancia e aprender uma li\\u00e7\\u00e3o e que cada pessoa qu\\u2026 https://t.co/Tp62LXTP5k\",\n",
      " \"17 Instagram accounts that demonstrate the importance of #feminism in 2018 https://t.co/RWU9oY5kNC\"\n",
      "]\n",
      "[\n",
      " \"facebook\",\n",
      " \"WomenThrive\",\n",
      " \"YouTube\",\n",
      " \"Nero\",\n",
      " \"MoscowTimes\"\n",
      "]\n",
      "[\n",
      " \"feminism\",\n",
      " \"MeToo\",\n",
      " \"equalpay\",\n",
      " \"genderinequality\",\n",
      " \"WEF2018\"\n",
      "]\n",
      "[\n",
      " \"Pakistani\",\n",
      " \"feminist\",\n",
      " \"banned\",\n",
      " \"by\",\n",
      " \"@Facebook\"\n",
      "]\n"
     ]
    }
   ],
   "source": [
    "status_texts = [ status['text'] \n",
    "                 for status in statuses ]\n",
    "\n",
    "screen_names = [ user_mention['screen_name'] \n",
    "                 for status in statuses\n",
    "                     for user_mention in status['entities']['user_mentions'] ]\n",
    "\n",
    "hashtags = [ hashtag['text'] \n",
    "             for status in statuses\n",
    "                 for hashtag in status['entities']['hashtags'] ]\n",
    "\n",
    "# Compute a collection of all words from all tweets\n",
    "words = [ w \n",
    "          for t in status_texts \n",
    "              for w in t.split() ]\n",
    "\n",
    "# Explore the first 5 items for each\n",
    "\n",
    "print (json.dumps(status_texts[0:5], indent=1))\n",
    "print (json.dumps(screen_names[0:5], indent=1))\n",
    "print (json.dumps(hashtags[0:5], indent=1))\n",
    "print (json.dumps(words[0:5], indent=1))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 93,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[('the', 33), ('#feminism', 33), ('RT', 23), ('a', 21), ('to', 18), ('of', 16), ('is', 16), ('on', 15), ('are', 13), ('as', 12)]\n",
      "[('MoscowTimes', 10), ('conservmillen', 3), ('Garymbparty', 2), ('facebook', 1), ('WomenThrive', 1), ('YouTube', 1), ('Nero', 1), ('cchukudebelu', 1), ('nazifumie', 1), ('H_Combs', 1)]\n",
      "[('feminism', 36), ('Feminism', 13), ('genderequality', 9), ('MeToo', 4), ('sexualharassment', 2), ('Women', 2), ('GenderEquality', 2), ('Canada', 2), ('equality', 2), ('equalpay', 1)]\n"
     ]
    }
   ],
   "source": [
    "#We make a table to count the frequency of words in related twits\n",
    "\n",
    "from collections import Counter\n",
    "\n",
    "for item in [words, screen_names, hashtags]:\n",
    "    c = Counter(item)\n",
    "    print (c.most_common()[:10]) # top 10\n",
    "    print"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 94,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "+-----------+-------+\n",
      "| Word      | Count |\n",
      "+-----------+-------+\n",
      "| the       |    33 |\n",
      "| #feminism |    33 |\n",
      "| RT        |    23 |\n",
      "| a         |    21 |\n",
      "| to        |    18 |\n",
      "| of        |    16 |\n",
      "| is        |    16 |\n",
      "| on        |    15 |\n",
      "| are       |    13 |\n",
      "| as        |    12 |\n",
      "+-----------+-------+\n",
      "+---------------+-------+\n",
      "| Screen Name   | Count |\n",
      "+---------------+-------+\n",
      "| MoscowTimes   |    10 |\n",
      "| conservmillen |     3 |\n",
      "| Garymbparty   |     2 |\n",
      "| facebook      |     1 |\n",
      "| WomenThrive   |     1 |\n",
      "| YouTube       |     1 |\n",
      "| Nero          |     1 |\n",
      "| cchukudebelu  |     1 |\n",
      "| nazifumie     |     1 |\n",
      "| H_Combs       |     1 |\n",
      "+---------------+-------+\n",
      "+------------------+-------+\n",
      "| Hashtag          | Count |\n",
      "+------------------+-------+\n",
      "| feminism         |    36 |\n",
      "| Feminism         |    13 |\n",
      "| genderequality   |     9 |\n",
      "| MeToo            |     4 |\n",
      "| sexualharassment |     2 |\n",
      "| Women            |     2 |\n",
      "| GenderEquality   |     2 |\n",
      "| Canada           |     2 |\n",
      "| equality         |     2 |\n",
      "| equalpay         |     1 |\n",
      "+------------------+-------+\n"
     ]
    }
   ],
   "source": [
    "#By using PrettyTable we can disploy the following tables so that we can interpret.\n",
    "\n",
    "for label, data in (('Word', words), \n",
    "                    ('Screen Name', screen_names), \n",
    "                    ('Hashtag', hashtags)):\n",
    "    pt = PrettyTable(field_names=[label, 'Count']) \n",
    "    c = Counter(data)\n",
    "    [ pt.add_row(kv) for kv in c.most_common()[:10] ]\n",
    "    pt.align[label], pt.align['Count'] = 'l', 'r' # Set column alignment\n",
    "    print (pt)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "For curiosity, we can also see whether people is saying some certain words. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 120,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 120,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Do people say the word \"violation\" when they talk about feminism?\n",
    "'violation' in ('Word', words)[1]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Apparently people do."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "## Conclusion\n",
    "\n",
    "Gender inequality has been a severe problem in India for centuries, even until today, we can still see unfortunate news like 'girls rape', 'violence against woman'. There are many statistical evidences could show how bad the situation is. Due to some limitations, we only extract two indicators from world bank dataset to give us a subtle but representative image of female status in India on work and education respectively. These two indicators have shown us that at least woman empowerment has been improved in recent years, no matter how bad it is initially.  \n",
    "\n",
    "Along with it, we can see from twitter data that 'feminism' has been a popular topic in India. The most interesting thing that turns out is the usage of hashtags where people clearly claim more equality by using them such as \"genderequality\", \"sexualHarassment\", \"MeToo\", \"equalpay\" etc. Regarding the screen names that are in the trending list, people is interacting with them where we can see international platforms like Facebook, YouTube or Moscow Times that are known to be very aware of the gender equality issue. \n",
    "\n",
    "Therefore, we conclude that Indian people do care about the issue of gender inequality and we can surely say that there is a claim or wish among twitter users to make real changes and make the gender parity more equal."
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
