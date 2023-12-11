# CIS-7-Final-Part-3
CIS 7 Final Project Part 

#include <iostream>
#include <vector>
#include <iomanip>
class Participant {
public:
    std::string name;
    std::string language;
    std::string specialization;
    std::string country;
    Participant(const std::string& n, const std::string& lang, const std::string& spec, const std::string& cntry)
        : name(n), language(lang), specialization(spec), country(cntry) {}
};
class Matchmaker {
public:
    static double calculateMatchProbability(const Participant& p1, const Participant& p2) {
        int commonAttributes = 0;
        if (p1.language == p2.language) commonAttributes++;
        if (p1.specialization == p2.specialization) commonAttributes++;
        if (p1.country == p2.country) commonAttributes++;
        return static_cast<double>(commonAttributes) / 3.0 * 100.0;
    }
    static std::pair<Participant, Participant> findMatch(const std::vector<Participant>& participants) {
        std::pair<Participant, Participant> bestMatch = {participants[0], participants[1]};
        double bestMatchProbability = calculateMatchProbability(participants[0], participants[1]);
        for (size_t i = 0; i < participants.size(); ++i) {
            for (size_t j = i + 1; j < participants.size(); ++j) {
                double currentProbability = calculateMatchProbability(participants[i], participants[j]);
                if (currentProbability > bestMatchProbability) {
                    bestMatch = {participants[i], participants[j]};
                    bestMatchProbability = currentProbability;
                }
            }
        }
        return bestMatch;
    }
};
int main() {
    std::vector<std::string> languages = {"English", "Spanish", "Mandarin", "Korean", "Tagalog"};
    std::vector<std::string> specializations = {"Anesthesiology", "Cardiology", "Family Medicine", "Emergency Medicine", "Internal Medicine"};
    std::vector<std::string> countries = {"United States of America", "China", "Mexico", "Canada", "Philippines", "Japan", "Spain", "England", "South Korea", "France"};
    std::vector<Participant> participants;
    for (int i = 0; i < 2; ++i) {
        std::string name;
        std::string language;
        std::string specialization;
        std::string country;
        std::cout << "Enter Participant " << (i + 1) << "'s name: ";
        std::cin >> name;
        std::cout << "Choose a language from the list (English, Spanish, Mandarin, Korean, Tagalog) for " << name << ": ";
        std::cin >> language;
        std::cout << "Choose a specialization from the list (Anesthesiology, Cardiology, Family Medicine, Emergency Medicine, Internal Medicine) for " << name << ": ";
        std::cin >> specialization;
        std::cout << "Choose a country from the list (United States of America, China, Mexico, Canada, Philippines, Japan, Spain, England, South Korea, France) for " << name << ": ";
        std::cin >> country;
        participants.emplace_back(name, language, specialization, country);
    }
    auto bestMatchedParticipants = Matchmaker::findMatch(participants);
    std::cout << "\nBest Matched Participants:\n";
    std::cout << "Participant 1: " << bestMatchedParticipants.first.name << " from " << bestMatchedParticipants.first.country << "\n";
    std::cout << "Participant 2: " << bestMatchedParticipants.second.name << " from " << bestMatchedParticipants.second.country << "\n";
    double matchProbability = Matchmaker::calculateMatchProbability(bestMatchedParticipants.first, bestMatchedParticipants.second);
    std::cout << "Match Probability: " << std::fixed << std::setprecision(2) << matchProbability << "%\n";
    return 0;
